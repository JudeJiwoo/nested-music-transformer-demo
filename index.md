<style>
.audio-table {
    display: block;
    overflow-x: auto;
    white-space: nowrap;
}

.audio-table table {
    width: auto;
    min-width: 150px; /* Adjust based on your needs */
}

.audio-table td, .audio-table th {
    min-width: 200px; /* Adjust based on the size of your audio players */
    border: 1px solid #ccc;
    text-align: center;
    padding: 8px;
}

.audio-table audio {
    width: 100%; /* Make audio player responsive within the cell */
    min-width: 250px; /* Adjust based on your needs */
}
</style>

[Jiwoo Ryu¹](https://sites.google.com/view/judejiwoo) &emsp;
[Hao-Wen Dong²](https://salu133445.github.io/) &emsp;
[Jongmin Jung¹](https://sakem.in/) &emsp;
[Dasaem Jeong¹](https://jdasam.github.io/maler/) &emsp;
<br>
¹SOGANG UNIVERSITY, ²UNIVERSITY OF CALIFORNIA SAN DIEGO
{:.center}

{% include icon_link.html text="paper" icon=site.icons.paper href="https://arxiv.org/abs/2408.01180.pdf" %} &emsp;
{% include icon_link.html text="video" icon=site.icons.video href="https://youtu.be/wBhvJX7Z8hY" %} &emsp;
{% include icon_link.html text="code" icon=site.icons.code href="https://github.com/JudeJiwoo/nmt" %} &emsp;
{:.center}

{% include youtube_player.html id="ebRg4T2A-W0" %}

<hr style="border: double 1.35px silver;">
## Content
---
- [NMT Architecture](#architecture)
- [Encoding Comparison](#Encoding)
- [Results](#Tables)
- [Generated Results](#generations)
  - [Best unconditioned generation samples](#best-unconditional)
  - [4-measure continuation comparison among models](#continuation)
  - [MAESTRO fine-tuned EnCodec token based continuated generation samples](#encodec)

<hr style="border: double 1.35px silver;">

## NMT Architecture {#architecture}
---
<img src="img/main_fig02.png" style="border: 2px solid grey">

Diagram of two prediction methods in sub-decoder.
{:.center .larger}
> __Note__: Our proposed Nested Music Transformer (NMT) predicts sub-tokens in a fully-sequential manner.

<img src="img/subdecoder_fig02.png" style="border: 2px solid grey" class="wider">

Illustrations of the proposed Nested Music Transformer (NMT) and other sub-decoder structures
{:.center .larger}
<br>

## Encoding Comparison {#Encoding}
---
<img src="img/encoding_fig02.png" style="border: 2px solid grey">

An example illustrating the proposed representations, note-based (NB) encoding (c) NB-Metric1st and (d) NB-Pitch1st, alongside REMI and Compound word. 
{:.center .larger}

> __Note__:  All encodings represent the same piece of music by using five musical features. Specifically, REMI and Compound word were not originally designed for multi-instrument pieces, which is why we renamed the encodings with “+I” to (a) and (b). Here, k denotes the number of notes and sequence length for NB, while r and c represent the ratios for REMI and Compound word, with values greater than 1.

<hr style="border: double 1.35px silver;">

## Results {#Tables}
---
<img src="tables/dataset_analysis.PNG" style="width:60%; border: 2px solid grey">
The statistics of the dataset used in the experiments.
{:.center .larger}
<br>

---
<img src="tables/param_dataset.PNG" style="width:50%; border: 2px solid grey">
The hyperparameters used in the experiments for each dataset.
{:.center .larger}

---
<img src="tables/main_table.PNG" style="width:80%; border: 2px solid grey">
The main results of the experiments for symbolic music generation. Comparison average NLL loss for each model.
{:.center .larger}

> __Note__: We used total 12 number of layers including number of main deocer layers, sub-decoder layers and feature-enricher layers in case it is utilized. All models are trained with 512 dimension of hidden size and 8 heads of multi-head attention.

## Generated Results {#generations}
---
### Best unconditioned generation samples {#best-unconditional}

> __Settings__: The results of unconditional generation from 4 different datasets. The model is given a random seed with only Start-of-Suence (SOS) token and generates the note sequences. The outputs are converted converted into MIDI file and then turned into audio file by Logic pro X (Digital Audio Workstation). We trimmed the audio file to have maximum 2 minutes of length.

<div class="audio-table" markdown="block">

|  | __SOD__{:.center} | __Lakh__{:.center} | __Pop1k7__{:.center} |
| --- | --- | --- | --- | --- |
| __REMI + flattening__ | {% include audio_player.html filename="audio/symbolic_uncond/sod/remi/first_sample.mp3" %} | {% include audio_player.html filename="audio/symbolic_uncond/lakh/remi/first_sample.mp3" %} | {% include audio_player.html filename="audio/symbolic_uncond/pop1k7/remi/first_sample.mp3" %} |
|  | {% include audio_player.html filename="audio/symbolic_uncond/sod/remi/second_sample.mp3" %} | {% include audio_player.html filename="audio/symbolic_uncond/lakh/remi/second_sample.mp3" %} | {% include audio_player.html filename="audio/symbolic_uncond/pop1k7/remi/second_sample.mp3" %} |
|  | {% include audio_player.html filename="audio/symbolic_uncond/sod/remi/third_sample.mp3" %} | {% include audio_player.html filename="audio/symbolic_uncond/lakh/remi/third_sample.mp3" %} | {% include audio_player.html filename="audio/symbolic_uncond/pop1k7/remi/third_sample.mp3" %}
| __NB-PF + NMT__ | {% include audio_player.html filename="audio/symbolic_uncond/sod/nb/first_sample.mp3" %} | {% include audio_player.html filename="audio/symbolic_uncond/lakh/nb/first_sample.mp3" %} | {% include audio_player.html filename="audio/symbolic_uncond/pop1k7/nb/first_sample.mp3" %} |
|  | {% include audio_player.html filename="audio/symbolic_uncond/sod/nb/second_sample.mp3" %} | {% include audio_player.html filename="audio/symbolic_uncond/lakh/nb/second_sample.mp3" %} | {% include audio_player.html filename="audio/symbolic_uncond/pop1k7/nb/second_sample.mp3" %} |
|  | {% include audio_player.html filename="audio/symbolic_uncond/sod/nb/third_sample.mp3" %} | {% include audio_player.html filename="audio/symbolic_uncond/lakh/nb/third_sample.mp3" %} | {% include audio_player.html filename="audio/symbolic_uncond/pop1k7/nb/third_sample.mp3" %}

</div>
---
### 4-measure continuation comparison among models in SOD dataset {#continuation}

> __Settings__: The model is provided with symbolic tokens of selected pieces, each comprising four measures in length. From these tokens, the model generates note sequences. The selection of pieces for the prompt is crucial, as captivating motifs would be provided for the continuation. Although the audio files were trimmed to a length of two minutes for demo page, during the listening test, the samples were limited to 50 seconds each. This adjustment was made to ensure that the total duration of the test remained at 20 minutes, thus helping participants maintain concentration.

<div class="audio-table" markdown="block">

|  | __Prompt 1__{:.center} | __Prompt 2__{:.center} | __Prompt 3__{:.center} |
| __REMI + flattening__ | {% include audio_player.html filename="audio/symbolic_continuation/101_remi.mp3" %} | {% include audio_player.html filename="audio/symbolic_continuation/165_remi.mp3" %} | {% include audio_player.html filename="audio/symbolic_continuation/181_remi.mp3" %} |
| __CP + Catvec__ | {% include audio_player.html filename="audio/symbolic_continuation/101_catvec.mp3" %} | {% include audio_player.html filename="audio/symbolic_continuation/165_catvec.mp3" %} | {% include audio_player.html filename="audio/symbolic_continuation/181_catvec.mp3" %} |
| __CP + NMT__ | {% include audio_player.html filename="audio/symbolic_continuation/101_cpnmt.mp3" %} | {% include audio_player.html filename="audio/symbolic_continuation/165_cpnmt.mp3" %} | {% include audio_player.html filename="audio/symbolic_continuation/181_cpnmt.mp3" %} | 
| __NB-PF + NMT__ | {% include audio_player.html filename="audio/symbolic_continuation/101_nb.mp3" %} | {% include audio_player.html filename="audio/symbolic_continuation/165_nb.mp3" %} | {% include audio_player.html filename="audio/symbolic_continuation/181_nb.mp3" %} |

</div>
---
### MAESTRO fine-tuned EnCodec token based continuated generation samples {#encodec}

> __Settings__: The model receives 10-second audio samples, encoded as EnCodec tokens. Six models are employed to generate tokens from these inputs. We then decoded the generated audio tokens into audio files using a decoder pretrained with the MAESTRO dataset.

<div class="audio-table" markdown="block">

|  | __Prompt 1__{:.center} | __Prompt 2__{:.center} | __Prompt 3__{:.center} |
| __Parallel__ | {% include audio_player.html filename="audio/encodec_continuation/1_00_Parallel.wav" %} | {% include audio_player.html filename="audio/encodec_continuation/3_00_Parallel.wav" %} | {% include audio_player.html filename="audio/encodec_continuation/4_00_Parallel.wav" %} |
| __Flatten__ | {% include audio_player.html filename="audio/encodec_continuation/1_01_Flatten.wav" %} | {% include audio_player.html filename="audio/encodec_continuation/3_01_Flatten.wav" %} | {% include audio_player.html filename="audio/encodec_continuation/4_01_Flatten.wav" %} |
| __Delay__ | {% include audio_player.html filename="audio/encodec_continuation/1_02_Delay.wav" %} | {% include audio_player.html filename="audio/encodec_continuation/3_02_Delay.wav" %} | {% include audio_player.html filename="audio/encodec_continuation/4_02_Delay.wav" %} |
| __Self-attention__ | {% include audio_player.html filename="audio/encodec_continuation/1_05_Self-attn.wav" %} | {% include audio_player.html filename="audio/encodec_continuation/3_05_Self-attn.wav" %} | {% include audio_player.html filename="audio/encodec_continuation/4_05_Self-attn.wav" %} |
| __Cross-attention__ | {% include audio_player.html filename="audio/encodec_continuation/1_06_Cross-attn.wav" %} | {% include audio_player.html filename="audio/encodec_continuation/3_06_Cross-attn.wav" %} | {% include audio_player.html filename="audio/encodec_continuation/4_06_Cross-attn.wav" %} |
| __NMT__ | {% include audio_player.html filename="audio/encodec_continuation/1_07_Enricher.wav" %} | {% include audio_player.html filename="audio/encodec_continuation/3_07_Enricher.wav" %} | {% include audio_player.html filename="audio/encodec_continuation/4_07_Enricher.wav" %} |

</div>
<hr style="border: double 1.35px silver;">
