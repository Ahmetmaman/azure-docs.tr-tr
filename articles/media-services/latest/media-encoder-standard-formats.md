---
title: Standart kodlayıcı biçimleri ve codec bileşenleri-Azure
description: Bu makale, Standardencoderönayar ile kullanabileceğiniz en yaygın içe aktarma ve dışa aktarma dosyası biçimlerinin bir listesini içerir.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/31/2020
ms.author: inhenkel
ms.reviewer: anilmur
ms.openlocfilehash: f6e9c0a3d9100bc447314f5874107363c6bf5d8a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89289452"
---
# <a name="standard-encoder-formats-and-codecs"></a>Standart kodlayıcı biçimleri ve codec bileşenleri

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Bu makale, [Standardencoderönayar](/rest/api/media/transforms/createorupdate#standardencoderpreset)ile kullanabileceğiniz en yaygın içe aktarma ve dışa aktarma dosyası biçimlerinin bir listesini içerir. **Standardencoderönayar**kullanarak özel ön ayarlar oluşturma hakkında bilgi için bkz. [özel önayar ile dönüşüm oluşturma](customize-encoder-presets-how-to.md).

## <a name="input-containerfile-formats"></a>Giriş kapsayıcısı/dosya biçimleri

| Dosya biçimleri (dosya uzantıları) | Desteklenir |
| --- | --- |
| FLV (H.264 ve AAC codec bileşenleriyle) (.flv) |Evet |
| MXF    (.mxf) |Evet |
| GXF    (.gxf) |Evet |
| MPEG2-PS, MPEG2-TS, 3GP (.ts, .ps, .3gp, .3gpp, .mpg) |Evet |
| Windows Media Video (WMV)/ASF (.wmv, .asf) |Evet |
| AVI (Sıkıştırılmamış 8 bit/10 bit) (.avi) |Evet |
| MP4 (.mp4, .m4a, .m4v)/ISMV (.isma, .ismv) |Evet |
| [Microsoft Digital Video Recording(DVR-MS)](/previous-versions/windows/desktop/mstv/about-the-dvr-ms-file-format) (.dvr-ms) |Evet |
| Matroska/WebM (.mkv) |Evet |
| WAVE/WAV (.wav) |Evet |
| QuickTime (.mov) |Evet |

### <a name="audio-formats-in-input-containers"></a>Giriş kapsayıcılarındaki ses biçimleri

Standart kodlayıcı, giriş kapsayıcılarında aşağıdaki ses biçimlerinin kullanılmasını destekler:

* Araya eklemeli stereo veya 5,1 örnekleri ile ses parçaları bulunan MXF, GXF ve QuickTime dosyaları

veya

* Sesin ayrı PCM parçaları olarak taşındığı, ancak kanal eşlemesinin (stero’ya veya 5.1’e) dosya meta verilerinden çıkarılabildiği MXF, GXF ve QuickTime dosyaları

## <a name="input-video-codecs"></a>Giriş video codec bileşenleri
| Giriş video codec bileşenleri | Desteklenir |
| --- | --- |
| AVC 8 bit/10 bit, 4:2:2’ye kadar, AVCIntra dahil |8 bit 4:2:0 ve 4:2:2 |
| Avid DNxHD (MXF biçiminde) |Evet |
| DVCPro/DVCProHD (MXF biçiminde) |Evet |
| Digital video (DV) (AVI dosyalarında) |Evet |
| JPEG 2000 |Evet |
| MPEG-2 (422 Profili ve Yüksek Düzeye kadar; XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ve D10 gibi çeşitler dahil) |422 Profiline kadar |
| MPEG-1 |Evet |
| VC-1/WMV9 |Evet |
| Canopus HQ/HQX |Hayır |
| MPEG-4 Bölüm 2 |Evet |
| [Theora](https://en.wikipedia.org/wiki/Theora) |Evet |
| YUV420 sıkıştırılmamış veya mezzanine |Evet |
| Apple ProRes 422 |Evet |
| Apple ProRes 422 LT |Evet |
| Apple ProRes 422 HQ |Evet |
| Apple ProRes Proxy |Evet |
| Apple ProRes 4444 |Evet |
| Apple ProRes 4444 XQ |Evet |
| HEVC/H. 265| Ana profil|

## <a name="input-audio-codecs"></a>Giriş sesi codec bileşenleri
| Giriş Ses Codec Bileşenleri | Desteklenir |
| --- | --- |
| AAC (AAC-LC, AAC-HE ve AAC-HEv2; 5.1’e kadar) |Evet |
| MPEG Katman 2 |Evet |
| MP3 (MPEG-1 Ses Katmanı 3) |Evet |
| Windows Media Ses |Evet |
| WAV/PCM |Evet |
| [FLAC](https://en.wikipedia.org/wiki/FLAC)</a> |Evet |
| [Opus](https://go.microsoft.com/fwlink/?LinkId=822667) |Evet |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |Evet |
| AMR (adaptive multi-rate) |Evet |
| AES (SMPTE 331M ve 302M, AES3-2003) |Hayır |
| Dolby® E |Hayır |
| Dolby® Digital (AC3) |Hayır |
| Dolby® Digital Plus (E-AC3) |Hayır |

## <a name="output-formats-and-codecs"></a>Çıkış biçimleri ve codec bileşenleri
Aşağıdaki tabloda, dışarı aktarma için desteklenen codec bileşenleri ve dosya biçimleri listelenmektedir.

| Dosya Biçimi | Video codec bileşeni | Ses codec bileşeni |
| --- | --- | --- |
| MP4 <br/><br/>(çoklu bit hızı MP4 kapsayıcıları dahil) |H. (yüksek, ana ve temel profiller) |AAC-LC, HE-AAC v1, HE-AAC v2 |
| MPEG2-TS |H. (yüksek, ana ve temel profiller) |AAC-LC, HE-AAC v1, HE-AAC v2 |

## <a name="next-steps"></a>Sonraki adımlar

[Özel önayar olan bir dönüşüm oluşturma](customize-encoder-presets-how-to.md)
