---
title: Web audio codec guide
slug: Web/Media/Formats/Audio_codecs
tags:
  - AAC
  - ALAC
  - Apple Lossless
  - Audio
  - Codecs
  - Compress
  - Decompress
  - Encoding
  - FLAC
  - Guide
  - Media
  - Opus
  - Vorbis
  - Web
  - data
  - decoding
  - formats
  - mp3
  - mp4
  - sound
---
<div>{{QuickLinksWithSubpages("/en-US/docs/Web/Media")}}</div>

<p>Even modest quality, high-fidelity stereo sound can use a substantial amount of disk space. For web developers, an even bigger concern is the network bandwidth needed in order to transfer audio, whether for streaming or to download it for use during gameplay. The processing of audio data to encode and decode it is handled by an audio <strong>{{Glossary("codec")}}</strong> (<strong>CO</strong>der/<strong>DEC</strong>oder). <span class="seoSummary">In this article, we look at audio codecs used on the web to compress and decompress audio, what their capabilities and use cases are, and offer guidance when choosing audio codecs to use for your content.</span></p>

<p>Additionally, WebRTC implementations generally use a subset of these codecs for their encoding and decoding of media, and may support additional codecs as well, for optimal cross-platform support of video and audio conferencing, and to integrate better with legacy telecommunication solutions. See <a href="/en-US/docs/Web/Media/Formats/WebRTC_codecs">Codecs used by WebRTC</a> for details.</p>

<p>For information about the fundamental concepts behind how digital audio works, see the article <a href="/en-US/docs/Web/Media/Formats/Audio_concepts">Digital audio concepts</a>.</p>

<h2 id="Common_codecs">Common codecs</h2>

<p>The list below denotes the codecs most commonly used on the web and which containers (file types) support them. If all you need to know is which codecs are even possible to use, this is for you. Of course, individual browsers may or may not choose to support all of these codecs, and their support for which container types can use them may vary as well. In addition, browsers may choose to support additional codecs not included on this list.</p>

<table class="standard-table">
 <thead>
  <tr>
   <th scope="row">Codec name (short)</th>
   <th scope="col">Full codec name</th>
   <th scope="col">Container support</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <th scope="row"><a href="#aac_advanced_audio_coding">AAC</a></th>
   <td>Advanced Audio Coding</td>
   <td><a href="/en-US/docs/Web/Media/Formats/Containers#mp4">MP4</a>, <a href="/en-US/docs/Web/Media/Formats/Containers#adts">ADTS</a>, <a href="/en-US/docs/Web/Media/Formats/Containers#3gp">3GP</a></td>
  </tr>
  <tr>
   <th scope="row"><a href="#alac_apple_lossless_audio_codec">ALAC</a></th>
   <td>Apple Lossless Audio Codec</td>
   <td><a href="/en-US/docs/Web/Media/Formats/Containers#mp4">MP4</a>, <a href="/en-US/docs/Web/Media/Formats/Containers#quicktime">QuickTime</a> (MOV)</td>
  </tr>
  <tr>
   <th scope="row"><a href="#amr_adaptive_multi-rate">AMR</a></th>
   <td>Adaptive Multi-Rate</td>
   <td><a href="/en-US/docs/Web/Media/Formats/Containers#3gp">3GP</a></td>
  </tr>
  <tr>
   <th scope="row"><a href="#flac_free_lossless_audio_codec">FLAC</a></th>
   <td>Free Lossless Audio Codec</td>
   <td><a href="/en-US/docs/Web/Media/Formats/Containers#mp4">MP4</a>, <a href="/en-US/docs/Web/Media/Formats/Containers#ogg">Ogg</a>, <a href="/en-US/docs/Web/Media/Formats/Containers#flac">FLAC</a></td>
  </tr>
  <tr>
   <th scope="row"><a href="#g.711_pulse_code_modulation_of_voice_frequencies">G.711</a></th>
   <td>Pulse Code Modulation (PCM) of Voice Frequencies</td>
   <td>{{Glossary("RTP")}} / <a href="/en-US/docs/Web/API/WebRTC_API">WebRTC</a></td>
  </tr>
  <tr>
   <th scope="row"><a href="#g.722_64_kbps_7_khz_audio_coding">G.722</a></th>
   <td>7 kHz Audio Coding Within 64 kbps (for telephony/{{Glossary("VoIP")}})</td>
   <td>{{Glossary("RTP")}} / <a href="/en-US/docs/Web/API/WebRTC_API">WebRTC</a></td>
  </tr>
  <tr>
   <th scope="row"><a href="#mp3_mpeg-1_audio_layer_iii">MP3</a></th>
   <td>MPEG-1 Audio Layer III</td>
   <td><a href="/en-US/docs/Web/Media/Formats/Containers#mp4">MP4</a>, <a href="/en-US/docs/Web/Media/Formats/Containers#adts">ADTS</a>, <a href="/en-US/docs/Web/Media/Formats/Containers#mpeg">MPEG</a>, <a href="/en-US/docs/Web/Media/Formats/Containers#3gp">3GP</a>
   <p>When MPEG-1 Audio Layer III codec data is stored in an MPEG file, and there is no video track on the file, the file is typically referred to as an MP3 file, even though it's still an MPEG format file.</p></td>
  </tr>
  <tr>
   <th scope="row"><a href="#opus">Opus</a></th>
   <td>Opus</td>
   <td><a href="/en-US/docs/Web/Media/Formats/Containers#webm">WebM</a>, <a href="/en-US/docs/Web/Media/Formats/Containers#mp4">MP4</a>, <a href="/en-US/docs/Web/Media/Formats/Containers#ogg">Ogg</a></td>
  </tr>
  <tr>
   <th scope="row"><a href="#vorbis">Vorbis</a></th>
   <td>Vorbis</td>
   <td><a href="/en-US/docs/Web/Media/Formats/Containers#webm">WebM</a>, <a href="/en-US/docs/Web/Media/Formats/Containers#ogg">Ogg</a></td>
  </tr>
 </tbody>
</table>

<h2 id="Factors_affecting_the_encoded_audio">Factors affecting the encoded audio</h2>

<p>There are two general categories of factors that affect the encoded audio which is output by an audio codec's encoder: details about the source audio's format and contents, and the codec and its configuration during the encoding process.</p>

<p>For each factor that affects the encoded audio, there is a simple rule that is nearly always true: because the fidelity of digital audio is determined by the granularity and precision of the samples taken to convert it into a data stream, the more data used to represent the digital version of the audio, the more closely the sampled sound will match the source material.</p>

<h3 id="The_effect_of_source_audio_format_on_encoded_audio_output">The effect of source audio format on encoded audio output</h3>

<p>Because encoded audio inherently uses fewer bits to represent each sample, the source audio format may actually have less impact on the encoded audio size than one might expect. However, a number of factors do still affect the encoded audio quality and size. The table below lists a number of key source audio file format factors and their impact on the encoded audio.</p>

<table class="standard-table">
 <caption>The effect of source audio format and contents on the encoded audio quality and size</caption>
 <thead>
  <tr>
   <th scope="row">Feature</th>
   <th scope="col">Effect on quality</th>
   <th scope="col">Effect on size</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <th scope="row"><a href="/en-US/docs/Web/Media/Formats/Audio_concepts#Audio_data_format_and_structure">Channel count</a></th>
   <td>The number of channels affects only the perception of directionality, not the quality.</td>
   <td>Each channel may substantially increase the encoded audio size, depending on contents and encoder settings.</td>
  </tr>
  <tr>
   <th scope="row">Noise / Hiss</th>
   <td>Unwanted background noise or hiss tends to reduce audio quality both directly (by masking details of the foreground audio) and indirectly (by making the audio waveform more complicated and therefore difficult to reduce in size while maintaining precision).</td>
   <td>Hiss, static, or background noise increases the audio complexity, which generally reduces the amount of compression which is possible.</td>
  </tr>
  <tr>
   <th scope="row"><a href="/en-US/docs/Web/Media/Formats/Audio_concepts#Sampling_audio">Sample rate</a></th>
   <td>The more samples available per second, the higher the resulting encoded audio fidelity is likely to be.</td>
   <td>Increasing the sample rate increases the encoded audio file's size.</td>
  </tr>
  <tr>
   <th scope="row"><a href="/en-US/docs/Web/Media/Formats/Audio_concepts#Audio_data_format_and_structure">Sample size</a></th>
   <td>The larger the samples, the more detail each sample can contain, resulting in more accurate representation of each sample.</td>
   <td>Depends on the codec; codecs typically have an internal sample format that may or may not be the same as the original sample size. But more source detail may make the encoded file larger; it will never make it smaller.</td>
  </tr>
 </tbody>
</table>

<p>Of course, these effects can be altered by decisions made while encoding the audio. For example, if the encoder is configured to reduce the sample rate, the sample rate's effect on the output file will be reduced in kind.</p>

<p>For more information about these and other features of audio data, see {{SectionOnPage("/en-US/docs/Web/Media/Formats/Audio_concepts", "Audio data format and structure")}}.</p>

<h3 id="The_effect_of_codec_configuration_on_encoded_audio_output">The effect of codec configuration on encoded audio output</h3>

<p>Audio codecs typically employ cleverly-designed and highly-complex mathematical algorithms to take source audio data and compress it to take substantially less space in memory or network bandwidth. In addition to choosing the type of encoder to use, you may have the opportunity to adjust the encoder using parameters that choose specific algorithms, tune those algorithms, and specify how many passes to apply while encoding.</p>

<table class="standard-table">
 <caption>Audio encoder configuration effects on quality and size</caption>
 <thead>
  <tr>
   <th scope="row">Feature</th>
   <th scope="col">Effect on quality</th>
   <th scope="col">Effect on size</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <th scope="row"><a href="/en-US/docs/Web/Media/Formats/Audio_concepts#lossy_vs_lossless_compression">Lossless compression</a></th>
   <td>No loss of fidelity</td>
   <td>Unlikely to get more than 40-50% compression</td>
  </tr>
  <tr>
   <th scope="row"><a href="/en-US/docs/Web/Media/Formats/Audio_concepts#lossy_vs_lossless_compression">Lossy compression</a></th>
   <td>Always some loss of fidelity; the higher the compression, the more the loss</td>
   <td>Compression of up to 80-95% possible</td>
  </tr>
  <tr>
   <th scope="row"><a href="/en-US/docs/Web/Media/Formats/Audio_concepts#quality">Quality setting</a></th>
   <td>The higher the quality, the better the fidelity of the encoded audio</td>
   <td>The higher the fidelity, the larger the resulting file becomes, though the amount of change varies from codec to codec</td>
  </tr>
  <tr>
   <th scope="row"><a href="/en-US/docs/Web/Media/Formats/Audio_concepts#bit_rate">Bit rate</a></th>
   <td>The higher the bit rate, the higher the quality can be</td>
   <td>The higher the bit rate, the larger the encoded file is likely to be</td>
  </tr>
  <tr>
   <th scope="row"><a href="/en-US/docs/Web/Media/Formats/Audio_concepts#audio_frequency_bandwidth">Audio frequency bandwidth</a></th>
   <td>If there is any audio in the removed frequency band(s), there may be a noticeable loss of fidelity</td>
   <td>Removing frequency bands means less data to encode, thus smaller encoded files</td>
  </tr>
  <tr>
   <th scope="row"><a href="/en-US/docs/Web/Media/Formats/Audio_concepts#stereo_coding_method">Stereo coding</a></th>
   <td>Simple stereo and <a href="/en-US/docs/Web/Media/Formats/Audio_concepts#mid-side_stereo_coding">mid-side stereo coding</a> don't affect quality; <a href="/en-US/docs/Web/Media/Formats/Audio_concepts#intensity_stereo_coding">intensity stereo coding</a> introduces loss of detail, however.</td>
   <td>Joint stereo can reduce the size of the encoded audio to some extent</td>
  </tr>
 </tbody>
</table>

<p>The parameters available—and the range of possible values—varies from codec to codec, and even among different encoding utilities for the same codec, so read the documentation that comes with the encoding software you use to learn more.</p>

<h3 id="Features_affecting_encoded_audio_size">Features affecting encoded audio size</h3>

<p>Several factors affect the size of the encoded audio. Some of these are a matter of the form of the source audio; others are related to decisions made while encoding the audio.</p>

<h4 id="Lossless_versus_lossy_codecs">Lossless versus lossy codecs</h4>

<p>There are two basic categories of audio compression. <strong>Lossless</strong> compression algorithms reduce the size of the audio without compromising the quality or fidelity of the sound. Upon decoding audio compressed with a lossless codec such as <a href="#flac_free_lossless_audio_codec">FLAC</a> or <a href="#alac_apple_lossless_audio_codec">ALAC</a>, the result is identical in every way to the original sound, down to the bit.</p>

<p><strong>Lossy</strong> codecs, on the other hand, take advantage of the fact that the human ear is not a perfect interpreter of audio, and of the fact that the human brain can pluck the important information out of imperfect or noisy audio. They strip away audio frequencies that aren't used much, tolerate loss of precision in the decoded output, and use other methods to lose audio content, quality, and fidelity to produce smaller encoded media. Upon decoding, the output is, to varying degrees, still understandable. The specific codec used—and the compression configuration selected—determine how close to the original, uncompressed audio signal the output seems to be when heard by the human ear.</p>

<p>Because of the differences in how lossy codecs work compared to lossless ones, especially the fact that lossless ones have to be much more conservative with their compression, lossy codecs nearly always result in significantly smaller compressed audio than lossless codecs do.</p>

<p>Generally speaking, the most common reasons to choose lossless audio are because you require archival-quality storage, or because the audio samples will be remixed and recompressed, and you wish to avoid the amplification of artifacts in the audio due to recompression. For real-time streaming of audio, a lossy codec is usually required in order to ensure the flow of data can keep up with the audio playback rate regardless of network performance.</p>

<h3 id="Maximum_number_of_channels">Maximum number of channels</h3>

<p>The audio delivered to each speaker in a sound system is provided by one audio channel in a stream. Monaural sound is a single channel. Stereo sound is two. 5.1 surround sound has five audio channels, plus one <strong>Low Frequency Enhancement</strong> (<strong>LFE</strong>) channel.</p>

<p>LFE channels are specifically designed to store low-frequency audio data, and are commonly used to provide audio data for subwoofers, for example. When you see the number of audio channels written in the form X.Y (such as 2.1 or 5.1), the number after the decimal point, Y, is the number of LFE channels. For example, MP3 supports one LFE channel, while AAC supports up to 16.</p>

<p>In addition to providing audio for specific speakers in a sound system, some codecs may allow audio channels to be used to provide alternative audio, such as vocals in different languages or descriptive audio for visually impaired people.</p>

<h3 id="Audio_frequency_bandwidth">Audio frequency bandwidth</h3>

<p>The <strong>audio frequency bandwidth</strong> of a codec indicates the range of audio frequencies that can be represented using the codec. Some codecs operate specifically by eliminating audio that falls outside a given frequency range. There is a correlation between the sample rate and the maxium sound frequency that can be represented by a waveform represented by a codec. At a theoretical level, the maximum frequency a codec can represent is the sample rate divided by two; this frequency is called the {{interwiki("wikipedia", "Nyquist frequency")}}. In reality, the maximum is slightly lower, but it's close.</p>

<p>The audio frequency bandwidth comes into play especially vividly when a codec is designed or configured to represent human speech rather than a broad range of sounds. Human speech generally resides within the audio frequency range of 300 Hz to 18 kHz. However, the vast majority of human vocalizations exist in the range 300 Hz to 8 kHz, and you can capture enough of human vocalizations in the frequency range 500 Hz to 3 kHz to still be understandable.</p>

<p>For that reason, speech-specific codecs often begin by dropping sound that falls outside a set range. That range is the audio frequency bandwidth. G.722, for example, strips away sounds outside the audio frequency bandwidth of 50 Hz to 7 kHz. This reduces the amount of data that needs to be encoded from the outset.</p>

<h2 id="Codec_details">Codec details</h2>

<p>Below we take a brief look at each of these codecs, looking at their basic capabilities and their primary use cases.</p>

<h3 id="aac_advanced_audio_coding">AAC (Advanced Audio Coding)</h3>

<p>The <strong>Advanced Audio Coding</strong> (<strong>AAC</strong>) codec is defined as part of the MPEG-4 (H.264) standard; specifically, as part of <a href="https://www.iso.org/standard/53943.html">MPEG-4 Part 3</a> and <a href="https://www.iso.org/standard/43345.html">MPEG-2 Part 7</a>. Designed to be able to provide more compression with higher audio fidelity than MP3, AAC has become a popular choice, and is the standard format for audio in many types of media, including Blu-Ray discs and HDTV, as well as being the format used for songs purchased from online vendors including iTunes.</p>

<p>AAC has a number of profiles that define methods of compressing audio for specific use cases, including everything from high quality surround sound to low-fidelity audio for speech-only use.</p>

<p>As a patent-encumbered format, AAC support is somewhat less predictable. For example, Firefox only supports AAC if support is provided by the operating system or an external library.</p>

<table class="standard-table">
 <tbody>
  <tr>
   <th scope="row">Supported bit rates</th>
   <td>Arbitrary, up to 512 kbps</td>
  </tr>
  <tr>
   <th scope="row">Variable Bit Rate (VBR) support</th>
   <td>Yes</td>
  </tr>
  <tr>
   <th scope="row">Supported sample formats</th>
   <td>32-bit integer</td>
  </tr>
  <tr>
   <th scope="row">Supported sample rates</th>
   <td>8 kHz - 96 kHz</td>
  </tr>
  <tr>
   <th scope="row">Recommended minimum bit rate for stereo sound</th>
   <td>96 kbps at 48 kHz sample rate</td>
  </tr>
  <tr>
   <th scope="row">Compression</th>
   <td>Lossy</td>
  </tr>
  <tr>
   <th scope="row">Maximum audio channels</th>
   <td>48 (plus 16 Low Frequency Enhancement channels)</td>
  </tr>
  <tr>
   <th scope="row">Audio frequency  bandwidth</th>
   <td>0 Hz - 96 kHz (standard audio channels)<br>
    0 Hz - 120 Hz (LFE channels)</td>
  </tr>
  <tr>
   <th scope="row">Latency</th>
   <td>20 ms to 405 ms</td>
  </tr>
  <tr>
   <th scope="row">Browser compatibility</th>
   <td>
    <table class="standard-table">
     <tbody>
      <tr>
       <th scope="row">Feature</th>
       <th scope="col">Chrome</th>
       <th scope="col">Edge</th>
       <th scope="col">Firefox</th>
       <th scope="col">Internet Explorer</th>
       <th scope="col">Opera</th>
       <th scope="col">Safari</th>
      </tr>
      <tr>
       <th scope="row">AAC support</th>
       <td>Yes</td>
       <td>Yes</td>
       <td>Yes</td>
       <td>9</td>
       <td>Yes</td>
       <td>3.1</td>
      </tr>
     </tbody>
    </table>
    <p>Due to patent issues, Firefox does not directly support AAC. Instead, Firefox relies upon a platform's native support for AAC. This capability was introduced on each platform in different Firefox releases:</p>
    <p>Chrome supports AAC only in MP4 containers, and only supports AAC's Main Profile. In addition, AAC is not available in Chromium builds.</p>
   </td>
  </tr>
  <tr>
   <th scope="row">Container support</th>
   <td>MP4, ADTS, 3GP</td>
  </tr>
  <tr>
   <th scope="row">{{Glossary("RTP")}} / <a href="/en-US/docs/Web/API/WebRTC_API">WebRTC</a> compatible</th>
   <td>Yes</td>
  </tr>
  <tr>
   <th scope="row">Licensing</th>
   <td>For streaming or distributing AAC-encoded content: no license required; developers of codecs are required to obtain a patent license through <a href="https://www.via-corp.com/licensing/aac/">VIA Licensing</a></td>
  </tr>
 </tbody>
</table>

<table class="standard-table" style="margin-left: 4em; max-width: 30em;">
 <caption>AAC support in Firefox using external library, by platform</caption>
 <thead>
  <tr>
   <th scope="row">Platform</th>
   <th scope="col">First Firefox version<br>
    with AAC support</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <th scope="row">Windows (Vista and later)</th>
   <td>22</td>
  </tr>
  <tr>
   <th scope="row">Android</th>
   <td>20</td>
  </tr>
  <tr>
   <th scope="row">Linux (depends on <a href="https://gstreamer.freedesktop.org/">GStreamer</a>)</th>
   <td>26</td>
  </tr>
  <tr>
   <th scope="row">macOS</th>
   <td>35</td>
  </tr>
 </tbody>
</table>

<h3 id="alac_apple_lossless_audio_codec">ALAC (Apple Lossless Audio Codec)</h3>

<p>The <strong>Apple Lossless Audio Codec</strong> (<strong>ALAC</strong> or <strong>Apple Lossless</strong>) is a lossless codec developed by Apple. After initially being a closed format, Apple opened it up under an Apache license.</p>

<p>Cross-platform and browser support for ALAC is not very strong, making it a less than ideal choice for general usage. However, if your target is primarily macOS and iOS users, it may be worth considering, as the operating systems have integrated support for ALAC. Otherwise, FLAC is likely a better choice, if you must use a lossless codec.</p>

<p>Keep in mind, however, that lossless codecs require substantially more bandwidth and storage capacity, and may not be a good choice outside very specific use cases.</p>

<table class="standard-table">
 <tbody>
  <tr>
   <th scope="row">Supported bit rates</th>
   <td>Based on the sample format and sample rate, as well as the compression level</td>
  </tr>
  <tr>
   <th scope="row">Variable Bit Rate (VBR) support</th>
   <td>No</td>
  </tr>
  <tr>
   <th scope="row">Supported sample formats</th>
   <td>16-bit, 20-bit, 24-bit, and 32-bit integer</td>
  </tr>
  <tr>
   <th scope="row">Supported sample rates</th>
   <td>1 Hz to 384,000 Hz</td>
  </tr>
  <tr>
   <th scope="row">Recommended minimum bit rate for stereo sound</th>
   <td>n/a</td>
  </tr>
  <tr>
   <th scope="row">Compression</th>
   <td>Lossless; up to 45-60%</td>
  </tr>
  <tr>
   <th scope="row">Maximum audio channels</th>
   <td>8 (up to 7.1 surround)</td>
  </tr>
  <tr>
   <th scope="row">Audio frequency bandwidth</th>
   <td>?</td>
  </tr>
  <tr>
   <th scope="row">Latency</th>
   <td>?</td>
  </tr>
  <tr>
   <th scope="row">Browser compatibility</th>
   <td>
    <table class="standard-table">
     <tbody>
      <tr>
       <th scope="row">Feature</th>
       <th scope="col">Chrome</th>
       <th scope="col">Edge</th>
       <th scope="col">Firefox</th>
       <th scope="col">Internet Explorer</th>
       <th scope="col">Opera</th>
       <th scope="col">Safari</th>
      </tr>
      <tr>
       <th scope="row">ALAC support</th>
       <td>No</td>
       <td>No</td>
       <td>No</td>
       <td>No</td>
       <td>No</td>
       <td>Yes</td>
      </tr>
     </tbody>
    </table>
   </td>
  </tr>
  <tr>
   <th scope="row">Container support</th>
   <td>MP4</td>
  </tr>
  <tr>
   <th scope="row">{{Glossary("RTP")}} / <a href="/en-US/docs/Web/API/WebRTC_API">WebRTC</a> compatible</th>
   <td>No</td>
  </tr>
  <tr>
   <th scope="row">Licensing</th>
   <td>Open license (Apache License 2.0); <a href="https://github.com/macosforge/alac">source code available on GitHub</a></td>
  </tr>
 </tbody>
</table>

<h3 id="AMR_Adaptive_Multi-Rate">AMR (Adaptive Multi-Rate)</h3>

<p>The <strong><a href="http://www.voiceage.com/AMR-NB.AMR.html">Adaptive Multi-Rate audio codec</a></strong> is optimized for encoding human speech efficiently. It was standardized in 1999 as part of the 3GPP audio standard used for both {{interwiki("wikipedia", "GSM")}} and {{interwiki("wikipedia", "UMTS")}} cellular telephony, and uses a multi-rate narrowband algorithm to encode audio frequencies at a telephony-grade quality level at around 7.4 kbps. In addition to being used for real-time telephony, AMR audio may be used for voicemail and other short audio recordings.</p>

<p>AMR audio which is stored in files may be typed <code>.amr</code>, but may also be encapsulated in <code>.3gp</code> files.</p>

<p>As a speech-specific codec, AMR is essentially useless for any other content, including audio containing only singing voices. Additionally, because AMR is designed to minimize capacity requirements, it only captures the portion of the full audio frequency bandwidth of human speech which is absolutely necessary to understand what's being said, so quality is reduced accordingly. If you need the ability to record audio at a minimal impact to network and/or storage capacity, AMR can be a great choice. However, if you need high-fidelity reproduction of human speech—or even low-quality music reproduction—you need to choose another format.</p>

<table class="standard-table">
 <tbody>
  <tr>
   <th rowspan="2" scope="row">Supported bit rates</th>
   <td><strong>Half Rate (HR) and Full Rate (FR):</strong> 1.8 kbps, 4.75 kbps, 5.15 kbpz, 5.9 kbps, 6.7 kbps, 7.4 kbps, 7.95 kbps</td>
  </tr>
  <tr>
   <td><strong>Full Rate (FR) only:</strong> 10.2 kbps and 12.2 kbps</td>
  </tr>
  <tr>
   <th scope="row">Variable Bit Rate (VBR) support</th>
   <td>No</td>
  </tr>
  <tr>
   <th scope="row">Supported sample formats</th>
   <td>13-bit integer</td>
  </tr>
  <tr>
   <th scope="row">Supported sample rates</th>
   <td>8 kHz</td>
  </tr>
  <tr>
   <th scope="row">Recommended minimum bit rate for stereo sound</th>
   <td>n/a</td>
  </tr>
  <tr>
   <th scope="row">Compression</th>
   <td>Lossy</td>
  </tr>
  <tr>
   <th scope="row">Maximum audio channels</th>
   <td>1</td>
  </tr>
  <tr>
   <th scope="row">Audio frequency  bandwidth</th>
   <td>200 Hz to 3,400 Hz</td>
  </tr>
  <tr>
   <th scope="row">Latency</th>
   <td>25 ms</td>
  </tr>
  <tr>
   <th scope="row">Browser compatibility</th>
   <td>
    <table class="standard-table">
     <tbody>
      <tr>
       <th scope="row">Feature</th>
       <th scope="col">Chrome</th>
       <th scope="col">Edge</th>
       <th scope="col">Firefox</th>
       <th scope="col">Internet Explorer</th>
       <th scope="col">Opera</th>
       <th scope="col">Safari</th>
      </tr>
      <tr>
       <th scope="row">AMR support</th>
       <td>No</td>
       <td>?</td>
       <td>No</td>
       <td>?</td>
       <td>No</td>
       <td>?</td>
      </tr>
     </tbody>
    </table>
    <p>While the Chrome browser does not support AMR, Chrome OS supports AMR-NB (narrowband) and AMR-WB (wideband).</p>
   </td>
  </tr>
  <tr>
   <th scope="row">Container support</th>
   <td>AMR, 3GPP</td>
  </tr>
  <tr>
   <th scope="row">{{Glossary("RTP")}} / <a href="/en-US/docs/Web/API/WebRTC_API">WebRTC</a> compatible</th>
   <td>No</td>
  </tr>
  <tr>
   <th scope="row">Licensing</th>
   <td>Non-free; license fees and annual royalties apply. See <a href="http://www.voiceage.com/Overview-lic.html">VoiceAge licensing</a> for details</td>
  </tr>
 </tbody>
</table>

<h3 id="flac_free_lossless_audio_codec">FLAC (Free Lossless Audio Codec)</h3>

<p><strong>FLAC </strong>(<strong>Free Lossless Audio Codec</strong>) is a lossless audio codec published by the <a href="https://xiph.org/">Xiph.org Foundation</a>. It provides good compression rates with no loss of audio fidelity; that is, the decompressed audio is identical to the original. Because the compression algorithm is specifically designed for audio, it gives better results than would be achieved using a general-purpose compression algorithm.</p>

<p>FLAC is a great choice for smaller audio effects files where pristine quality and tonal accuracy are desired, as well as for archival of music.</p>

<table class="standard-table">
 <tbody>
  <tr>
   <th scope="row">Supported bit rates</th>
   <td>—</td>
  </tr>
  <tr>
   <th scope="row">Variable Bit Rate (VBR) support</th>
   <td>No</td>
  </tr>
  <tr>
   <th scope="row">Supported sample formats</th>
   <td>4-bit to 24-bit integers</td>
  </tr>
  <tr>
   <th scope="row">Supported sample rates</th>
   <td>1 Hz to 65,535 Hz (in 1 Hz increments) or 10 Hz to 655,350 Hz in 10 Hz increments</td>
  </tr>
  <tr>
   <th scope="row">Recommended minimum bit rate for stereo sound</th>
   <td>—</td>
  </tr>
  <tr>
   <th scope="row">Compression</th>
   <td>Lossless; up to 40-50% size reduction</td>
  </tr>
  <tr>
   <th scope="row">Maximum audio channels</th>
   <td>8</td>
  </tr>
  <tr>
   <th scope="row">Audio frequency  bandwidth</th>
   <td>Full-spectrum</td>
  </tr>
  <tr>
   <th scope="row">Latency</th>
   <td>4.3 ms to 92 ms with 46.4 ms being the typical average</td>
  </tr>
  <tr>
   <th scope="row">Browser compatibility</th>
   <td>
    <table class="standard-table">
     <tbody>
      <tr>
       <th scope="row">Feature</th>
       <th scope="col">Chrome</th>
       <th scope="col">Edge</th>
       <th scope="col">Firefox</th>
       <th scope="col">Internet Explorer</th>
       <th scope="col">Opera</th>
       <th scope="col">Safari</th>
      </tr>
      <tr>
       <th scope="row">FLAC support</th>
       <td>Yes</td>
       <td>Yes</td>
       <td>51 (desktop)<br>
        58 (mobile)</td>
       <td>No</td>
       <td>No</td>
       <td>11</td>
      </tr>
     </tbody>
    </table>
   </td>
  </tr>
  <tr>
   <th scope="row">Container support</th>
   <td>MP4, Ogg, FLAC</td>
  </tr>
  <tr>
   <th scope="row">{{Glossary("RTP")}} / <a href="/en-US/docs/Web/API/WebRTC_API">WebRTC</a> compatible</th>
   <td>No</td>
  </tr>
  <tr>
   <th scope="row">Licensing</th>
   <td>Fully open and free of any licensing requirements</td>
  </tr>
 </tbody>
</table>

<h3 id="g.711_pulse_code_modulation_of_voice_frequencies">G.711 (Pulse Code Modulation of Voice Frequencies)</h3>
<p>The <strong>G.711</strong> specification, published by the International Telecommunications Union (ITU), was issued in 1972 to define standard audio encoding for telephone applications. It supports voice-grade audio covering frequencies from 300 to 3400 Hz. It is used extensively for telephone traffic and voicemail, and it is the highest quality audio encoding which can be transmitted through the public telephone network.</p>

<p>G.711 is not a high fidelity codec, but is instead optimized to support a wide range of voice levels (from whisper to shout) while maintaining high intelligibility and low computational complexity. G.711 uses a logarithmic companding algorithm which provides 14 bits of dynamic range in an 8-bit sample. It uses a sampling rate of 8000 samples/sec, corresponding to a bitrate of 64000 bps.</p>

<p>There are two flavors of G.711 which indicate the exact mathematical equation for the algorithm: {{interwiki("wikipedia", "M-law", "µ-law")}} (commonly used in North America and Japan) and {{interwiki("wikipedia", "A-law")}} (common in the rest of the world). There is no substantial quality difference between the two laws, and it is simple to transcode audio from one to the other. Nevertheless, it is important to specify which law is in use in any replay application or file format. A-law audio will replay poorly if mistakenly uncompressed with the µ-law algorithm, and viceversa.</p>

<p>This codec is required to be supported by all <a href="/en-US/docs/Web/API/WebRTC_API">WebRTC</a> solutions because it is simple, easy to implement, widely-used, and broadly compatible across all modern computing platforms.</p>
<table class="standard-table">
 <tbody>
  <tr>
   <th scope="row">Supported bit rates</th>
   <td>64 kbps</td>
  </tr>
  <tr>
   <th scope="row">Variable Bit Rate (VBR) support</th>
   <td>No</td>
  </tr>
  <tr>
   <th scope="row">Supported sample formats</th>
   <td>encoded audio is 8 bits per sample</td>
  </tr>
  <tr>
   <th scope="row">Supported sample rates</th>
   <td>8 kHz</td>
  </tr>
  <tr>
   <th scope="row">Recommended minimum bit rate for stereo sound</th>
   <td>128 kbps</td>
  </tr>
  <tr>
   <th scope="row">Compression</th>
   <td>Logarithmic companding (µ-law or A-law)</td>
  </tr>
  <tr>
   <th scope="row">Maximum audio channels</th>
   <td>2</td>
  </tr>
  <tr>
   <th scope="row">Audio frequency bandwidth</th>
   <td>300 Hz – 3400 Hz</td>
  </tr>
  <tr>
   <th scope="row">Latency</th>
   <td>0.125 ms</td>
  </tr>
  <tr>
   <th scope="row">Browser compatibility</th>
   <td>
    <table class="standard-table">
     <tbody>
      <tr>
       <th scope="row">Feature</th>
       <th scope="col">Chrome</th>
       <th scope="col">Edge</th>
       <th scope="col">Firefox</th>
       <th scope="col">Internet Explorer</th>
       <th scope="col">Opera</th>
       <th scope="col">Safari</th>
      </tr>
      <tr>
       <th scope="row">G.711 support</th>
       <td>23</td>
       <td>15</td>
       <td>22</td>
       <td>No</td>
       <td>43</td>
       <td>11</td>
      </tr>
     </tbody>
    </table>
    <p>G.711 is supported only for WebRTC connections.</p>
   </td>
  </tr>
  <tr>
   <th scope="row">Container support</th>
   <td>3GP, WAV</td>
  </tr>
  <tr>
   <th scope="row">{{Glossary("RTP")}} / <a href="/en-US/docs/Web/API/WebRTC_API">WebRTC</a> compatible</th>
   <td>Yes</td>
  </tr>
  <tr>
   <th scope="row">Licensing</th>
   <td>All applicable patents have expired, so G.711 is free to use without restriction</td>
  </tr>
 </tbody>
</table>

<h3 id="g.722_64_kbps_7_khz_audio_coding">G.722 (64 kbps (7 kHz) audio coding)</h3>

<p>Published by the International Telecommunications Union (ITU), the <strong>G.722</strong> codec is designed specifically for voice compression. Its audio coding bandwidth is limited to the 50 Hz to 7,000 Hz range, which covers most of the frequency range of typical human vocalization. This makes it ill-suited for handling any audio that might range outside the human speech range, such as music.</p>

<p>G.722 audio is encoded using Adaptive Differential Pulse Code Modulation (ADPCM), in which each sample is represented not by its absolute value, but as a value indicating how much the new sample differs from the previous sample.</p>

<p>G.722 is primarily used with WebRTC connections, as it's one of the audio codecs mandated by the WebRTC specification.</p>

<table class="standard-table">
 <tbody>
  <tr>
   <th scope="row">Supported bit rates</th>
   <td>G.722: 48 kbps, 56 kbps, and 64 kbps; however, in practice 64 kbps is always used<br>
    G.722 Annex B Super Wide-Band: 64 kbps, 80 kbps, and 96 kbps<br>
    G.722 Annex D Stereo Wide-Band: 64 kbps and 80 kbps<br>
    G.722 Annex D Stereo Super Wide-Band: 80 kbps, 96 kbps, 112 kbps, and 128 kbps</td>
  </tr>
  <tr>
   <th scope="row">Variable Bit Rate (VBR) support</th>
   <td>No</td>
  </tr>
  <tr>
   <th scope="row">Supported sample formats</th>
   <td>14-bit integer</td>
  </tr>
  <tr>
   <th scope="row">Supported sample rates</th>
   <td>16 kHz (ADPCM is specified to allow 8 kHz, 11.025 kHz, 22.05 kHz, 44.1 kHz, but G.722 uses 16 kHz)</td>
  </tr>
  <tr>
   <th scope="row">Recommended minimum bit rate for stereo sound</th>
   <td>128 kbps at 44.1 kHz sample rate</td>
  </tr>
  <tr>
   <th scope="row">Compression</th>
   <td>Lossy</td>
  </tr>
  <tr>
   <th scope="row">Maximum audio channels</th>
   <td>2</td>
  </tr>
  <tr>
   <th scope="row">Audio frequency bandwidth</th>
   <td>50 Hz - 7 kHz</td>
  </tr>
  <tr>
   <th scope="row">Latency</th>
   <td>4 ms</td>
  </tr>
  <tr>
   <th scope="row">Browser compatibility</th>
   <td>
    <table class="standard-table">
     <tbody>
      <tr>
       <th scope="row">Feature</th>
       <th scope="col">Chrome</th>
       <th scope="col">Edge</th>
       <th scope="col">Firefox</th>
       <th scope="col">Internet Explorer</th>
       <th scope="col">Opera</th>
       <th scope="col">Safari</th>
      </tr>
      <tr>
       <th scope="row">G.722 support</th>
       <td>Yes</td>
       <td>Yes</td>
       <td>Yes</td>
       <td>No</td>
       <td>Yes</td>
       <td>Yes</td>
      </tr>
     </tbody>
    </table>
    <p>WebRTC only.</p>
   </td>
  </tr>
  <tr>
   <th scope="row">Container support</th>
   <td>3GP, AMR-WB</td>
  </tr>
  <tr>
   <th scope="row">{{Glossary("RTP")}} / <a href="/en-US/docs/Web/API/WebRTC_API">WebRTC</a> compatible</th>
   <td>Yes</td>
  </tr>
  <tr>
   <th scope="row">Licensing</th>
   <td>All applicable patents have expired; G.722 is free to use without restriction</td>
  </tr>
 </tbody>
</table>

<h3 id="mp3_mpeg-1_audio_layer_iii">MP3 (MPEG-1 Audio Layer III)</h3>

<p>Of the audio formats specified by the MPEG/MPEG-2 standards, <strong>MPEG-1 Audio Layer III</strong>—otherwise known as <strong>{{interwiki("wikipedia", "MP3")}}</strong>—is by far the most widely used and best-known. The MP3 codec is defined by <a href="https://www.iso.org/standard/22412.html">MPEG-1 Part 3</a> and <a href="https://www.iso.org/standard/26797.html">MPEG-2 Part 3</a>, and was introduced in 1991 (and finalized in 1992).</p>

<p>When MP3-format audio is stored inside an MPEG container, the resulting file is also referred to as just an "MP3 file" or "MP3." Files with the ubiquitous <code>.mp3</code> extension are stored in what is perhaps the most widely distributed audio file format in the world, which is in large part responsible for the digital audio revolution of the late 1990s and early 2000s.</p>

<p>MPEG-1 MP3 audio supports higher bit rates as well as higher sampling rates than MP3 audio in MPEG-2 files. MPEG-1 format MP3 is generally best for music or other complex audio, while MPEG-2 mode MP3 audio is acceptable for speech and other simpler sounds.</p>

<p>The patents behind MP3 have expired, removing many or most licensing concerns around using MP3 files in your projects. That makes them a good choice for many projects.</p>

<table class="standard-table">
 <tbody>
  <tr>
   <th rowspan="2" scope="row">Supported bit rates</th>
   <td><strong>MPEG-1 mode:</strong> 32 kbps, 40 kbps, 48 kbps, 56 kbps, 64 kbps, 80 kbps, 96 kbps, 112 kbps, 128 kbps, 160 kbps, 192 kbps, 224 kbps, 256 kbps, 320 kbps</td>
  </tr>
  <tr>
   <td><strong>MPEG-2 mode:</strong> 8 kbps, 16 kbps, 24 kbps, 32 kbps, 40 kbps, 48 kbps, 56 kbps, 64 kbps, 80 kbps, 96 kbps, 112 kbps, 128 kbps, 144 kbps, 160 kbps</td>
  </tr>
  <tr>
   <th scope="row">Variable Bit Rate (VBR) support</th>
   <td>Yes</td>
  </tr>
  <tr>
   <th scope="row">Supported sample formats</th>
   <td>16-bit integer</td>
  </tr>
  <tr>
   <th rowspan="2" scope="row">Supported sample rates</th>
   <td><strong>MPEG-1 mode:</strong> 32000 Hz, 44100 Hz, 48000 Hz</td>
  </tr>
  <tr>
   <td><strong>MPEG-2 mode:</strong> 16000 Hz, 22050 Hz, 24000 Hz (Half the frequency of the MPEG-1 supported modes)</td>
  </tr>
  <tr>
   <th scope="row">Recommended minimum bit rate for stereo sound</th>
   <td>128 kbps at 48 kHz sample rate</td>
  </tr>
  <tr>
   <th scope="row">Compression</th>
   <td>Lossy</td>
  </tr>
  <tr>
   <th rowspan="2" scope="row">Maximum audio channels</th>
   <td><strong>MPEG-1 mode:</strong> 2 [2.0]</td>
  </tr>
  <tr>
   <td><strong>MPEG-2 mode:</strong> 5 (plus 1 optional Low Frequency Enhancement channel) [5.1]</td>
  </tr>
  <tr>
   <th scope="row">Audio frequency bandwidth</th>
   <td>Varies, depending on bit rate and psychoacoustic analysis</td>
  </tr>
  <tr>
   <th scope="row">Latency</th>
   <td>At least 100 ms</td>
  </tr>
  <tr>
   <th scope="row">Browser compatibility</th>
   <td>
    <table class="standard-table">
     <tbody>
      <tr>
       <th scope="row">Feature</th>
       <th scope="col">Chrome</th>
       <th scope="col">Edge</th>
       <th scope="col">Firefox</th>
       <th scope="col">Internet Explorer</th>
       <th scope="col">Opera</th>
       <th scope="col">Safari</th>
      </tr>
      <tr>
       <th scope="row">MP3 support</th>
       <td>Yes</td>
       <td>Yes</td>
       <td>Yes</td>
       <td>9</td>
       <td>Yes</td>
       <td>3.1</td>
      </tr>
     </tbody>
    </table>
   </td>
  </tr>
  <tr>
   <th scope="row">Container support</th>
   <td>MPEG-1, MPEG-2, MP4, ADTS, 3GP</td>
  </tr>
  <tr>
   <th scope="row">{{Glossary("RTP")}} / <a href="/en-US/docs/Web/API/WebRTC_API">WebRTC</a> compatible</th>
   <td>No</td>
  </tr>
  <tr>
   <th scope="row">Licensing</th>
   <td>Patent-free in the EU as of 2012; patent-free in the United States as of April 16, 2017; now free to use</td>
  </tr>
 </tbody>
</table>

<p>For patent reasons, Firefox did not directly support MP3 prior to version 71; instead, platform-native libraries were used to support MP3. This capability was introduced on each platform in different Firefox releases:</p>

<table class="standard-table" style="margin-left: 4em; max-width: 30em;">
 <caption>MP3 support with external library, by platform, in Firefox</caption>
 <thead>
  <tr>
   <th scope="row">Platform</th>
   <th scope="col">First Firefox version<br>
    with MP3 support</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <th scope="row">Windows (Vista and later)</th>
   <td>22</td>
  </tr>
  <tr>
   <th scope="row">Android</th>
   <td>20</td>
  </tr>
  <tr>
   <th scope="row">Linux (depends on <a href="https://gstreamer.freedesktop.org/">GStreamer</a>)</th>
   <td>26</td>
  </tr>
  <tr>
   <th scope="row">macOS</th>
   <td>35</td>
  </tr>
 </tbody>
</table>

<h3 id="opus">Opus</h3>

<p>The {{interwiki("wikipedia", "Opus (audio format)", "Opus")}} audio format was created by the Xiph.org Foundation as a fully open audio format; it has been standardized by <a href="https://www.ietf.org/">IETF</a> as {{RFC(6716)}}. It's a good general-purpose audio codec that can efficiently handle both low-complexity audio such as speech as well as music and other high-complexity sounds.</p>

<p>Opus supports multiple compression algorithms, and can even use more than one algorithm in the same audio file, since the encoder can choose the bit rate, audio bandwidth, algorithm, and other details of the compression settings for each frame of audio.</p>

<p>Opus is a good all-around audio codec for use in your web applications, and can be used for any audio tasks you have in mind.</p>

<table class="standard-table">
 <tbody>
  <tr>
   <th scope="row">Supported bit rates</th>
   <td>6 kbps - 510 kbps</td>
  </tr>
  <tr>
   <th scope="row">Variable Bit Rate (VBR) support</th>
   <td>Yes</td>
  </tr>
  <tr>
   <th scope="row">Supported sample formats</th>
   <td>16-bit integer and 32-bit float (-1.0 to 1.0)</td>
  </tr>
  <tr>
   <th scope="row">Supported sample rates</th>
   <td>
    <table class="standard-table">
     <tbody>
      <tr>
       <th scope="row">Profile</th>
       <th scope="col">Effective sample rate</th>
      </tr>
      <tr>
       <th scope="row">Narrowband (NB)</th>
       <td>8 kHz</td>
      </tr>
      <tr>
       <th scope="row">Medium band (MB)</th>
       <td>12 kHz</td>
      </tr>
      <tr>
       <th scope="row">Wideband (WB)</th>
       <td>16 kHz</td>
      </tr>
      <tr>
       <th scope="row">Super Wideband (SWB)</th>
       <td>24 kHz</td>
      </tr>
      <tr>
       <th scope="row">Fullband (FB)</th>
       <td>48 kHz</td>
      </tr>
     </tbody>
    </table>
    <p>The specified sample rates are <em>effective sample rates</em>. Opus uses an algorithm based on audio bandwidths rather than sample rates. See {{RFC(6716, "", 2)}} for details. In addition, there is an <em>optional</em> part of the Opus specification (Opus Custom) that does allow for non-standard sample rates, but the use of this feature is discouraged.</p>
   </td>
  </tr>
  <tr>
   <th scope="row">Recommended minimum bit rate for stereo sound</th>
   <td>96 kbps at 48 kHz sample rate</td>
  </tr>
  <tr>
   <th scope="row">Compression</th>
   <td>Lossy</td>
  </tr>
  <tr>
   <th scope="row">Maximum audio channels</th>
   <td>255 (up to 1 LFE channel)</td>
  </tr>
  <tr>
   <th scope="row">Audio frequency bandwidth</th>
   <td>
    <table class="standard-table">
     <thead>
      <tr>
       <th scope="row">Profile</th>
       <th scope="col">Audio bandwidth</th>
      </tr>
     </thead>
     <tbody>
      <tr>
       <th scope="row">Narrowband (NB)</th>
       <td>4 kHz</td>
      </tr>
      <tr>
       <th scope="row">Medium band (MB)</th>
       <td>6 kHz</td>
      </tr>
      <tr>
       <th scope="row">Wideband (WB)</th>
       <td>8 kHz</td>
      </tr>
      <tr>
       <th scope="row">Super Wideband (SWB)</th>
       <td>12 kHz</td>
      </tr>
      <tr>
       <th scope="row">Fullband (FB)</th>
       <td>20 kHz</td>
      </tr>
     </tbody>
    </table>
    <p>Although the {{interwiki("wikipedia", "Nyquist–Shannon sampling theorem")}} shows that audio bandwidth can be as much as one half of the sampling rate, Opus doesn't allow encoding outside a maximum 20 kHz audio frequency band, since the human ear can't percieve anything above the 20 kHz point anyway. This saves some space in the encoded audio.</p>
   </td>
  </tr>
  <tr>
   <th scope="row">Latency</th>
   <td>5 ms to 66.5 ms</td>
  </tr>
  <tr>
   <th scope="row">Browser compatibility</th>
   <td>
    <table class="standard-table">
     <tbody>
      <tr>
       <th scope="row">Feature</th>
       <th scope="col">Chrome</th>
       <th scope="col">Edge</th>
       <th scope="col">Firefox</th>
       <th scope="col">Internet Explorer</th>
       <th scope="col">Opera</th>
       <th scope="col">Safari</th>
      </tr>
      <tr>
       <th scope="row">Opus support</th>
       <td>33</td>
       <td>14</td>
       <td>15</td>
       <td>No</td>
       <td>20</td>
       <td>11</td>
      </tr>
     </tbody>
    </table>
    <p>This information refers to support for Opus in HTML {{HTMLElement("audio")}} and {{HTMLElement("video")}} elements, and not to WebRTC.</p>

    <p>Safari supports Opus in the {{HTMLElement("audio")}} element only when packaged in a CAF file, and only on macOS High Sierra (10.13) or iOS 11.</p>
   </td>
  </tr>
  <tr>
   <th scope="row">Container support</th>
   <td>Ogg, WebM, MPEG-TS, MP4</td>
  </tr>
  <tr>
   <th scope="row">{{Glossary("RTP")}} / <a href="/en-US/docs/Web/API/WebRTC_API">WebRTC</a> compatible</th>
   <td>Yes</td>
  </tr>
  <tr>
   <th scope="row">Licensing</th>
   <td>Fully open and free of any licensing requirements</td>
  </tr>
 </tbody>
</table>

<h3 id="vorbis">Vorbis</h3>

<p><a href="https://www.xiph.org/vorbis/">Vorbis</a> is an open format from the <a href="https://xiph.org/">Xiph.org Foundation</a> which supports a wide range of channel combinations, including monaural, stereo, polyphonic, quadraphonic, 5.1 surround, ambisonic, or up to 255 discrete audio channels. Depending on the quality setting used during encoding, the resulting bit rate can vary from around 45 kbps to 500 kbps. Vorbis inherently uses variable bit rate encoding; the bit rate can vary from one sample to the next as needed during the compression process.</p>

<p>Generally, Vorbis is more efficient in terms of size and bit rate than MP3 at similar quality levels. This and its free and open license, make it a good choice for many kinds of audio data as long as its high latency isn't a problem.</p>

<table class="standard-table">
 <tbody>
  <tr>
   <th scope="row">Supported bit rates</th>
   <td>45 kbps - 500 kbps</td>
  </tr>
  <tr>
   <th scope="row">Variable Bit Rate (VBR) support</th>
   <td>Yes</td>
  </tr>
  <tr>
   <th scope="row">Supported sample formats</th>
   <td>16-bit integer</td>
  </tr>
  <tr>
   <th scope="row">Supported sample rates</th>
   <td>8 kHz - 192 kHz</td>
  </tr>
  <tr>
   <th scope="row">Recommended minimum bit rate for stereo sound</th>
   <td>192 kbps at 48 kHz; this is typically accomplished by setting the quality level to 6 to 8.</td>
  </tr>
  <tr>
   <th scope="row">Compression</th>
   <td>Lossy</td>
  </tr>
  <tr>
   <th scope="row">Maximum audio channels</th>
   <td>255</td>
  </tr>
  <tr>
   <th scope="row">Audio frequency bandwidth</th>
   <td></td>
  </tr>
  <tr>
   <th scope="row">Latency</th>
   <td>At least 100 ms</td>
  </tr>
  <tr>
   <th scope="row">Browser compatibility</th>
   <td>
    <table class="standard-table">
     <tbody>
      <tr>
       <th scope="row">Feature</th>
       <th scope="col">Chrome</th>
       <th scope="col">Edge</th>
       <th scope="col">Firefox</th>
       <th scope="col">Internet Explorer</th>
       <th scope="col">Opera</th>
       <th scope="col">Safari</th>
      </tr>
      <tr>
       <th scope="row">Vorbis support</th>
       <td>4</td>
       <td>17</td>
       <td>3.5</td>
       <td>No</td>
       <td>11.5</td>
       <td>No</td>
      </tr>
     </tbody>
    </table>

    <p>This information refers to support for Vorbis in HTML {{HTMLElement("audio")}} and {{HTMLElement("video")}} elements, and not to WebRTC.</p>
   </td>
  </tr>
  <tr>
   <th scope="row">Container support</th>
   <td>Ogg, WebM</td>
  </tr>
  <tr>
   <th scope="row">{{Glossary("RTP")}} / <a href="/en-US/docs/Web/API/WebRTC_API">WebRTC</a> compatible</th>
   <td>Yes</td>
  </tr>
  <tr>
   <th scope="row">Licensing</th>
   <td>Fully open and free of any licensing requirements</td>
  </tr>
 </tbody>
</table>

<h2 id="Choosing_an_audio_codec">Choosing an audio codec</h2>

<p>Typically, regardless of what codec you use, it will generally get the job done, even if it's not the ideal choice, as long as you choose a codec not specifically designed for a totally different kind of source audio. Choosing a voice-only codec and trying to use it for music will not give you usable results, for instance.</p>

<p>Some codecs may limit compatibility, however, and others may be more optimal for your needs than others. Here we'll provide guidance to help you select an appropriate codec for your use case.</p>

<p>When choosing a codec to use for your audio, you should first consider the following questions:</p>

<ul>
 <li>Will the encoded audio be getting remixed or recompressed? If so, avoid lossy compression, which would be compounded by recompressing the audio; or at least use as little compression as possible.</li>
 <li>If the audio needs to go into a specific file type, keep that in mind, since media containers typically support a specific subset of the available codecs.</li>
 <li>What kind of audio content will the codec be handling? Certain codecs are specifically designed for voice-only content (they take advantage of the reduced frequency range needed for human speech). Others may have an algorithmic tendency to perform worse when encoding specific genres of music.</li>
 <li>What bit rates and other configurable properties does each codec have that may make it a good (or poor) choice?</li>
 <li>To what extent, if at all, does latency matter for your needs? If you need sound that is very precisely timed, the lower the latency the better.</li>
 <li>How much compression do you need to achieve?</li>
</ul>

<p>Let's consider a few common scenarios to get a feel for the decision-making process.</p>

<h3 id="Example_Music_for_streaming">Example: Music for streaming</h3>

<p>For streaming music, you want to select a codec that minimizes bandwidth usage as much as possible while introducing as few artifacts as possible into the audio through compression. This is necessary because the rate at which the music is downloaded needs to be no greater than the amount of bandwidth available on the network, and ideally there should be room left for network speed fluctuations and use of the network by other applications.</p>

<p>Unless there's a specific need for lossless compression, or the network bandwidth is guaranteed to be high enough to support it, a lossy compression scheme is a good choice. Which one you choose depends on browser compatibility and the availability of any special features you may need the codec to support.</p>

<p><em>Usually</em> the latency is not particularly important when streaming music. Possible exceptions include looped music, where you need music to be able to play back uninterrupted over and over, or when you need to be able to play songs back to back with no gap between them. This can be particularly important for classical music, theatrical soundtracks, and for background music during gameplay.</p>

<p>For general music playback, the three most likely candidates are MP3, AAC, and Vorbis.</p>

<ul>
 <li>AAC in an MP4 container is supported by all major browsers, which makes this a great choice.</li>
 <li>Vorbis is almost always used in Ogg files, but Ogg containers are not universally supported. Even Microsoft Edge, which supports both Vorbis, does not yet support Ogg containers.</li>
 <li>
  <p>MP3 (MPEG-1 Audio Layer III) is supported by all major browsers. These files are MPEG-1 files that contain an Audio Layer III track.</p>
 </li>
</ul>

<p>If you need to minimize latency during music playback, you should strongly consider Opus, which has the lowest range of latencies of the general-purpose codecs (5 ms to 66.5 ms, compared to at least 100 ms for the others).</p>

<div class="notecard note">
  <p><strong>Note:</strong> Compatibility information described here is generally correct as of the time this article was written; however, there may be caveats and exceptions. Be sure to refer to the compatibility tables before committing to a given media format.</p>
</div>

<p>Based on this, AAC is likely your best choice if you can only support one audio format. Of course, if you can provide multiple formats (for example, by using the {{HTMLElement("source")}} element within your {{HTMLElement("audio")}} and {{HTMLElement("video")}} elements), you can avoid many or all of those exceptions.</p>

<h3 id="Example_Music_for_download">Example: Music for download</h3>

<p>Music being downloaded by the user can be compressed to a larger overall file size than streamed music, since (unlike when streaming) it doesn't matter if the download speed is slower than the playback speed of the media. That means you can consider using lossy compression at higher bit rates, which results in larger files but with less loss of fidelity. Or you can choose a lossless format. The choice depends largely on the union of your application's requirements and the preferences of your users.</p>

<p>For an actual music download service, you might offer songs for download as 128 Kbps MP3 files, 256 kbps AAC files (in MP4 containers), or FLAC files, depending on a preference selected by the user. If you need to choose just one format, select one that makes sense given your requirements and the type of audio content being downloaded.</p>

<p>Generally, of course, MP3 is the most common format used for music; choose a bit rate of at least 192 kbps if possible. The iTunes store, on the other hand, distributes music in 256 kbps AAC format.</p>

<h3 id="Example_Voice_recording_and_playback">Example: Voice recording and playback</h3>

<p>The specific characteristics of human speech allow speech-specific codecs to compress the audio far more than most general-purpose codecs can. That's because although humans hear frequencies ranging from around 20 Hz to 20,000 Hz, and human speech sounds range from about 300 Hz to 18,000 Hz, the majority of the speech sounds that we need in order to understand what's being said lie in the frequency range of 500 Hz to 3,000 Hz or so. That means voice-only codecs can throw away everything else.</p>

<p>The voice-specific codecs are all inherently very lossy, however, and any sound with significant information in the frequency bands outside the vocal range captured will be entirely lost. This makes these codecs completely unsuitable for anything but spoken words. Even audio that contains only voices, but singing rather than speaking, will likely not be of acceptable quality in one of these formats.</p>

<p>Voice recording and playback usually needs to be low-latency in order to synchronize with video tracks, or in order to avoid cross-talk or other problems. Fortunately, the characteristics that lead to speech codecs being so efficient storage space-wise also make them tend to be very low latency. If you're working with WebRTC, <a href="#g.722_64_kbps_7_khz_audio_coding">G.722</a>, for example, has 4 ms latency (compared with over 100 ms for MP3), and <a href="#amr_adaptive_multi-rate">AMR</a>'s latency is around 25 ms.</p>

<div class="notecard note">
<p><strong>Note:</strong> For more about WebRTC and the codecs it can use, see <a href="/en-US/docs/Web/Media/Formats/WebRTC_codecs">Codecs used by WebRTC</a>.</p>
</div>

<p>The codecs generally used on the web that are used for voice-only encoding are G.722 and AMR. AMR is a narrowband codec, encoding only the frequencies between 200 Hz and 3,400 Hz at bit rates typically around 7.4 kbps, while G.722 is a wideband codec that expands the audio bandwidth to 50 Hz to 7,000 Hz at much higher bit rates—usually 64 kbps.</p>

<p>If you have plenty of network bandwidth to work with, and are reasonably certain your users will as well, G.722 is the better choice. To maximize the storage and network efficiency in a constrained environment, choose AMR.</p>

<h3 id="Example_Audio_clips_for_professional_mixing">Example: Audio clips for professional mixing</h3>

<p>When compressing audio that will be getting mixed or remixed, you typically want zero or close to zero loss of fidelity, which suggests a lossless codec may be in order. However, because lossless encoding naturally has a much lower compression level than lossy encoding, you may find that if your source audio is large enough, you might have to choose a lossy encoder anyway, especially in a web environment in which you can't control the download rate of the media.</p>

<p>Assuming that lossless compression is our best option here (which it usually is, as long as the audio files are small), the three strongest candidates from a codec perspective are {{interwiki("wikipedia", "FLAC")}}, {{interwiki("wikipedia", "Apple Lossless", "Apple Lossless (ALAC)")}}, and {{interwiki("wikipedia", "Audio Lossless Coding", "MPEG-4 ALS")}}. Which we choose will depend on browser support and which media container formats support them.</p>

<p>For the purposes of this example, we'll assume that all browsers have the same codec and container support as Firefox (although this is far from true). Consider the breadth of actual support for the codecs when making your decisions.</p>

<ul>
 <li>Firefox supports FLAC in FLAC's native containers as well as in Ogg and MPEG-4 (MP4) files.</li>
 <li>Firefox only supports Apple Lossless through its platform-specific QuickTime support.</li>
 <li>Firefox doesn't support MP4 ALS.</li>
</ul>

<p>In this case, it appears that FLAC is most likely the best codec to go with; ALAC has little to no direct browser support.</p>

<h2 id="Audio_encoding_software">Audio encoding software</h2>

<p>There are many tools available for encoding audio. The simplest are those which are meant for ripping CDs or pulling in audio files and quickly and automatically converting them to MP3 or AAC format to store in a library, such as <a href="https://www.apple.com/itunes">iTunes</a>. But when developing web apps that use audio as a component of the app, such as games, you will need more control over the encoding process and more options surrounding the format used while encoding the audio.</p>

<p>A few popular options:</p>

<dl>
 <dt><a href="https://ffmpeg.org/">FFmpeg</a></dt>
 <dd>Arguably the best-known and most widely regarded open source codec packages available, FFmpeg supports the majority of the most popular audio formats and provides command-line tools and libraries for encoding, decoding, and performing format conversions of both audio and video. Binaries are available for macOS, Linux, and Windows.</dd>
 <dt><a href="https://handbrake.fr/">Handbrake</a></dt>
 <dd>A highly popular open source front-end to FFmpeg which adds a graphical user interface that makes it much easier to control the wide variety of options FFmpeg offers while encoding audio and/or video. Binaries are available for macOS, Linux, and Windows.</dd>
 <dt><a href="https://www.audacityteam.org/">Audacity</a></dt>
 <dd>An open source audio editor that supports loading audio from many different formats, editing, filtering, and adjusting the audio, and saving it back out in either the original format or a new format.  Available for macOS, Linux, and Windows.</dd>
 <dt><a href="http://lame.sourceforge.net/">LAME</a></dt>
 <dd>A high quality open source MP3 encoder with support for CBR, ABR, and VBR encoding as well as a variety of other options. Distributed only in source form by the LAME project, but can be installed using <a href="https://brew.sh/">Homebrew</a> or similar tools.</dd>
</dl>

<h2 id="See_also">See also</h2>

<ul>
 <li><a href="/en-US/docs/Web/Media/Formats/Containers">Media container formats</a></li>
 <li>The {{HTMLElement("audio")}} and {{HTMLElement("video")}} elements</li>
 <li><a href="/en-US/docs/Web/API/WebRTC_API">WebRTC API</a></li>
 <li><a href="/en-US/docs/Web/Media/Formats/Video_codecs">Web video codec guide</a></li>
 <li><a href="/en-US/docs/Web/Media/Formats/WebRTC_codecs">Codecs used by WebRTC</a></li>
</ul>