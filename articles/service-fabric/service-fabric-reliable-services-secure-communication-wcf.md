---
title: Azure Service Fabric WCF tabanlı hizmet iletişimi güvenli | Microsoft Docs
description: Bir Azure Service Fabric kümede çalışan güvenilir hizmetler için WCF tabanlı iletişimleri güvenli hale getirmeye öğrenin.
services: service-fabric
documentationcenter: .net
author: suchiagicha
manager: timlt
editor: vturecek
ms.assetid: fc129c1a-fbe4-4339-83ae-0e69a41654e0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 04/20/2017
ms.author: suchiagicha
ms.openlocfilehash: 1304bfd376fca7eb55c4533a690a12c3fef16efc
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34205748"
---
# <a name="secure-wcf-based-communications-for-a-service"></a>Bir hizmet için WCF tabanlı iletişimleri güvenli
Güvenlik iletişim en önemli yönlerinden birisidir. Güvenilir hizmetler uygulama çerçevesi birkaç önceden oluşturulmuş iletişimi yığınları ve güvenliği geliştirmek için kullanabileceğiniz araçlar sağlar. Bu makalede hizmet remoting kullanırken güvenliğini artırmak hakkında alınmaktadır.

Var olan kullanıyoruz [örnek](service-fabric-reliable-services-communication-wcf.md) , güvenilir hizmetler için bir WCF tabanlı iletişim yığını ayarlanacağı açıklanmaktadır. Bir WCF tabanlı iletişim yığını kullanırken bir hizmeti güvenli hale getirmek için aşağıdaki adımları izleyin:

1. Hizmeti için WCF iletişimi dinleyicisi güvenli hale getirmek gereken (`WcfCommunicationListener`), oluşturduğunuz. Bunu yapmak için değiştirmeniz `CreateServiceReplicaListeners` yöntemi.

    ```csharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        return new[]
        {
            new ServiceReplicaListener(
                this.CreateWcfCommunicationListener)
        };
    }

    private WcfCommunicationListener<ICalculator> CreateWcfCommunicationListener(StatefulServiceContext context)
    {
       var wcfCommunicationListener = new WcfCommunicationListener<ICalculator>(
            serviceContext:context,
            wcfServiceObject:this,
            // For this example, we will be using NetTcpBinding.
            listenerBinding: GetNetTcpBinding(),
            endpointResourceName:"WcfServiceEndpoint");

        // Add certificate details in the ServiceHost credentials.
        wcfCommunicationListener.ServiceHost.Credentials.ServiceCertificate.SetCertificate(
            StoreLocation.LocalMachine,
            StoreName.My,
            X509FindType.FindByThumbprint,
            "9DC906B169DC4FAFFD1697AC781E806790749D2F");
        return wcfCommunicationListener;
    }

    private static NetTcpBinding GetNetTcpBinding()
    {
        NetTcpBinding b = new NetTcpBinding(SecurityMode.TransportWithMessageCredential);
        b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;
        return b;
    }
    ```
2. İstemci `WcfCommunicationClient` önceki oluşturulduğu sınıf [örnek](service-fabric-reliable-services-communication-wcf.md) değişmeden kalır. Ancak geçersiz kılmanız gerekir `CreateClientAsync` yöntemi `WcfCommunicationClientFactory`:

    ```csharp
    public class SecureWcfCommunicationClientFactory<TServiceContract> : WcfCommunicationClientFactory<TServiceContract> where TServiceContract : class
    {
        private readonly Binding clientBinding;
        private readonly object callbackObject;
        public SecureWcfCommunicationClientFactory(
            Binding clientBinding,
            IEnumerable<IExceptionHandler> exceptionHandlers = null,
            IServicePartitionResolver servicePartitionResolver = null,
            string traceId = null,
            object callback = null)
            : base(clientBinding, exceptionHandlers, servicePartitionResolver,traceId,callback)
        {
            this.clientBinding = clientBinding;
            this.callbackObject = callback;
        }

        protected override Task<WcfCommunicationClient<TServiceContract>> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
        {
            var endpointAddress = new EndpointAddress(new Uri(endpoint));
            ChannelFactory<TServiceContract> channelFactory;
            if (this.callbackObject != null)
            {
                channelFactory = new DuplexChannelFactory<TServiceContract>(
                this.callbackObject,
                this.clientBinding,
                endpointAddress);
            }
            else
            {
                channelFactory = new ChannelFactory<TServiceContract>(this.clientBinding, endpointAddress);
            }
            // Add certificate details to the ChannelFactory credentials.
            // These credentials will be used by the clients created by
            // SecureWcfCommunicationClientFactory.  
            channelFactory.Credentials.ClientCertificate.SetCertificate(
                StoreLocation.LocalMachine,
                StoreName.My,
                X509FindType.FindByThumbprint,
                "9DC906B169DC4FAFFD1697AC781E806790749D2F");
            var channel = channelFactory.CreateChannel();
            var clientChannel = ((IClientChannel)channel);
            clientChannel.OperationTimeout = this.clientBinding.ReceiveTimeout;
            return Task.FromResult(this.CreateWcfCommunicationClient(channel));
        }
    }
    ```

    Kullanım `SecureWcfCommunicationClientFactory` WCF iletişimi istemcisi oluşturmak için (`WcfCommunicationClient`). İstemci, hizmet yöntemleri çağırmak için kullanın.

    ```csharp
    IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();

    var wcfClientFactory = new SecureWcfCommunicationClientFactory<ICalculator>(clientBinding: GetNetTcpBinding(), servicePartitionResolver: partitionResolver);

    var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
        wcfClientFactory,
        ServiceUri,
        ServicePartitionKey.Singleton);

    var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
        client => client.Channel.Add(2, 3)).Result;
    ```

Sonraki adım olarak, okuma [Reliable Services OWIN ile Web API](service-fabric-reliable-services-communication-webapi.md).
