<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.10.2/dist/katex.min.css" integrity="sha384-yFRtMMDnQtDRO8rLpMIKrtPCD5jdktao2TV19YiZYWMDkUR5GQZR/NOVTdquEx1j" crossorigin="anonymous">
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.10.2/dist/katex.min.js" integrity="sha384-9Nhn55MVVN0/4OFx7EE5kpFBPsEMZxKTCnA+4fqDmg12eCTqGi6+BB2LjY8brQxJ" crossorigin="anonymous"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.10.2/dist/contrib/auto-render.min.js" integrity="sha384-kWPLUVMOks5AQFrykwIup5lo0m3iMkkHrD0uJ4H5cjeGihAutqP0yW0J6dpFiVkI" crossorigin="anonymous" onload="renderMathInElement(document.body);"></script>

<style>
audio{
  width:200px;
  height:40px;
}
audio::-webkit-media-controls-timeline,
audio::-webkit-media-controls-seek-back-button,
audio::-webkit-media-controls-seek-forward-button {
  display: none !important;
}

.diag{
  display: block;
  margin-left: auto;
  margin-right: auto;
  /* width: 60%; */
  height:200px;
}
</style>

# Implicit Controls in Text-To-Music Diffusion Models

This is the accompanying website to "Implicit Controls in Text-To-Music Diffusion Models".

  * [System Overview](#systems-overview)
  * [Sound Examples](#sound-examples)
      - [Inpainting](#inpainting)
      - [Text-Audio Interpolations](#interpolations)
      - [Zero-Shot Slider](#slider)
      - [Stereo width](#stereo-width)
      - [Variations](#variations)
      - [Loop Sampling](#loop-sampling)
  * [Ethics Statement](#ethics-statement)
  * [Extra References](#extra-references)

## System Overview 

<figure>
  <img src="https://sonycslparis.github.io/diffariff-nime-companion/diff-a-riff-website.png" alt="Overview of Diff-A-Riff"/>
  <!-- <figcaption> <b>Overview of Diff-A-Riff.</b> The CAE Encoder transforms the music context into a compressed representation, concatenated with a noisy sample, and further processed through the multi-scale U-Net. The generated latent sequence is decoded into audio via the CAE Decoder.</figcaption> -->
</figure>



Diff-A-Riff is a Latent Diffusion Model capable of generating instrumental accompaniments for any musical audio context. 

It relies on a pretrained consistency model-based Autoencoder (CAE) and a generative model operating on its latent embeddings. The LDM follows the framework of Elucidated Diffusion Models (EDMs).

Given a pair of input context and target accompaniment audio segments, the model is trained to reconstruct the accompaniment given the context and a CLAP embedding derived from a randomly selected sub-segment of the target itself. At inference time, Diff-A-Riff allows to generate single instrument tracks under different conditioning signals.
- First, a user can choose to provide a context, which is a piece of music that the generated material has to fit into. If provided, the context is encoded by the CAE to give a sequence of latents that we call $$\textit{Context}$$. When a context provided, we talk of accompaniment generation instead of single instrument generation.
- Then, the user can also rely on CLAP-derived embedings to further specify the material to be generated. CLAP provides a multimodal embedding space shared between audio and text modalities. This means that the user can provide either a music reference or a text prompt, which after being encoded in CLAP give $$\textit{CLAP}_\text{A}$$ and $$\textit{CLAP}_\text{T}$$ respectively.

## Sound Examples
In this section, we demonstrate the implicit controls of Diff-A-Riff.

### Inpainting
As done traditionally with diffusion models, Diff-A-Riff can be used to perform audio inpainting. At each denoising step, the region to be kept is replaced with the corresponding noisy region of the final output, while the inpainted region is denoised normally. This way, we can enforce the inpainted region to blend well with the surroundings.

In line with most diffusion models, Diff-A-Riff allows to perform audio inpainting. During each denoising iteration, the inpainted area undergoes standard denoising, while the region to keep is substituted with its noisy counterpart from the final output. This approach ensures seamless integration of the inpainted section with its surroundings.
In the following examples, all tracks are inpainted from second 5 to 8.

<table class="tg">
<thead>
  <tr>
    <th class="tg-0pky">Context</th>
    <th class="tg-0lax">Masked Original Accompaniment</th>
    <!-- <th class="tg-0lax" colspan="4">Inpaintings</th> -->
    <th class="tg-0pky">Inpainting w/ Mix</th>
    <th class="tg-0lax">Inpainting Solo</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-0lax" rowspan="3"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/1/context.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax" rowspan="3"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/1/original_masked.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/1/inpainting0_mix.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/1/inpainting0.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/1/inpainting1_mix.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/1/inpainting1.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/1/inpainting2_mix.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/1/inpainting2.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
<!-- new ctx -->
  <tr>
    <td class="tg-0lax" rowspan="3"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/2/context.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax" rowspan="3"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/2/original_masked.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/2/inpainting0_mix.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/2/inpainting0.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/2/inpainting1_mix.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/2/inpainting1.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/2/inpainting2_mix.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/2/inpainting2.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
<!-- new ctx -->
  <tr>
    <td class="tg-0lax" rowspan="3"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/3/context.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax" rowspan="3"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/3/original_masked.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/3/inpainting0_mix.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/3/inpainting0.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/3/inpainting1_mix.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/3/inpainting1.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/3/inpainting2_mix.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/inpainting/3/inpainting2.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
<!-- new ctx -->
</tbody>
</table>

### Text-Audio Interpolation

<img src="https://sonycslparis.github.io/diffariff-nime-companion/diags/interpolations.png" alt="Interpolations"/>

We can interpolate between different references in the CLAP space. Here, we demonstrate the impact of a interpolation between an audio-derived CLAP embedding and a text-derived one, with an interpolation ratio $$r$$.

<table class="tg">
<tbody>
  <tr>
    <th class="tg-0pky"> Audio Reference </th>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/2/ref.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/3/ref.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/4/ref.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/5/ref.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <th class="tg-0pky"> $$ r = 0 $$ </th>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/2/gen0.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/3/gen0.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/4/gen0.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/5/gen0.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <th class="tg-0pky"> $$ r = 0.2 $$ </th>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/2/gen0.2.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/3/gen0.2.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/4/gen0.2.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/5/gen0.2.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>

  <tr>
    <th class="tg-0pky"> $$ r = 0.4 $$ </th>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/2/gen0.4.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/3/gen0.4.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/4/gen0.4.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/5/gen0.4.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <th class="tg-0pky"> $$ r = 0.6 $$ </th>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/2/gen0.6.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/3/gen0.6.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/4/gen0.6.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/5/gen0.6.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <th class="tg-0pky"> $$ r = 0.8 $$ </th>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/2/gen0.8.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/3/gen0.8.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/4/gen0.8.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/5/gen0.8.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <th class="tg-0pky"> $$ r = 1 $$ </th>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/2/gen1.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/3/gen1.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/4/gen1.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/interpolations/5/gen1.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
    <tr>
    <th class="tg-0pky"> Text Prompt </th>
    <td class="tg-0pky"> "Latin Percussion" </td>
    <td class="tg-0pky"> "Oriental Percussion Texture" </td>
    <td class="tg-0pky"> "The Cathciest Darbouka Rhythm" </td>
    <td class="tg-0pky"> "Rhythm on Huge Industrial Metal Plates" </td>
  </tr>
  
</tbody>
</table>

### Zero-Shot Slider

We can create a slider to control a property of the generation described through text.

<table class="tg">
<tbody>
  <tr>
    <th class="tg-0pky"> Context </th>
    <!-- <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/1/context.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td> -->
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/2/context.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/3/context.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/4/context.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/5/context.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <th class="tg-0pky"> Text/Audio Reference </th>
    <td class="tg-0pky"> "electric guitar" </td>
    <td class="tg-0pky"> "percussive drum rhythm with kick and snare" </td>
    <td class="tg-0pky"> "acoustic guitar" </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/5/reference.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <th class="tg-0pky"> Result </th>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/2/gen0.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/3/gen0.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/4/gen0.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/5/gen0.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <th class="tg-0pky"> Left slider prompt </th>
    <td class="tg-0pky"> "soft, sweet and tranquil" </td>
    <td class="tg-0pky"> "funny childrens toys" </td>
    <td class="tg-0pky"> "sad and mellow" </td>
    <td class="tg-0pky"> "legato, sustained notes" </td>
  </tr>
  <tr>
    <th class="tg-0pky"> <img src="https://sonycslparis.github.io/diffariff-nime-companion/diags/slider-1.png" alt="Slider position"/> </th>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/2/gen1.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/3/gen1.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/4/gen1.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/5/gen1.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <th class="tg-0pky"> <img src="https://sonycslparis.github.io/diffariff-nime-companion/diags/slider-25.png" alt="Slider position"/> </th>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/2/gen2.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/3/gen2.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/4/gen2.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/5/gen2.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>

  <tr>
    <th class="tg-0pky"> <img src="https://sonycslparis.github.io/diffariff-nime-companion/diags/slider-75.png" alt="Slider position"/> </th>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/2/gen3.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/3/gen3.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/4/gen3.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/5/gen3.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <th class="tg-0pky"> <img src="https://sonycslparis.github.io/diffariff-nime-companion/diags/slider-100.png" alt="Slider position"/> </th>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/2/gen4.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/3/gen4.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/4/gen4.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0pky"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/slider/5/gen4.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <th class="tg-0pky"> Right slider prompt </th>
    <td class="tg-0pky"> "hard hitting aggressive" </td>
    <td class="tg-0pky"> "deep and ominous"                  </td>
    <td class="tg-0pky"> "happy and delirious"               </td>
    <td class="tg-0pky"> "staccato, short accentuated notes" </td>
  </tr>

</tbody>
</table>

### Variations

<img class="diag" src="https://sonycslparis.github.io/diffariff-nime-companion/diags/variation.png" alt="Variation Generation"/>

Given an audio file, we can encode it in the CAE latent space and get the corresponding latent sequence. By adding noise to it, and denoising this noisy sequence again, we end up with a variation of the first sequence. We can then decode it to obtain a variation of the input audio. The amount of noise added is controlled through the variation strength parameter $$s_\text{Var}$$, which allows to control how different to the original a variation can be.

<table class="tg">
<thead>
  <tr>
    <th class="tg-0pky">Reference</th>
    <th class="tg-0lax">$$s_\text{Var} = 0.2$$</th>
    <th class="tg-0lax">$$0.5$$</th>
    <th class="tg-0lax">$$0.8$$</th>

  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/variations/1/orig.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/variations/1/0.20.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/variations/1/0.50.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/variations/1/0.80.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/variations/2/orig.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/variations/2/0.20.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/variations/2/0.50.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/variations/2/0.80.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/variations/3/orig.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/variations/3/0.20.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/variations/3/0.50.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/variations/3/0.80.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/variations/4/orig.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/variations/4/0.20.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/variations/4/0.50.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/variations/4/0.80.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>

</tbody>
</table>


### Stereo width

<img src="https://sonycslparis.github.io/diffariff-nime-companion/diags/stereo.png" alt="Pseudo Stereo Generation"/>

Following the same principle as for variations, for any mono signal, we can create a slight variation of it and use the original and the variation as left and right channels, creating what we call pseudo-stereo. Here, you can find examples of pseudo stereo files, generated from different stereo width $$s_\text{Stereo}$$.

<table class="tg">
<thead>
  <tr>
    <th class="tg-0pky"> $$s_\text{Stereo} = 0$$ </th>
    <th class="tg-0lax">$$0.2$$</th>
    <th class="tg-0lax">$$0.4$$</th>
    <th class="tg-0lax">$$0.5$$</th>

  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/stereo/1/0.0.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/stereo/1/0.2.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/stereo/1/0.4.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/stereo/1/0.5.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/stereo/2/0.0.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/stereo/2/0.2.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/stereo/2/0.4.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/stereo/2/0.5.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
    <tr>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/stereo/3/0.0.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/stereo/3/0.2.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/stereo/3/0.4.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/stereo/3/0.5.mp3" type="audio/wav"> Your Browser does not support the audio tag </audio> </td>
  </tr>
</tbody>
</table>

### Loop Sampling

<img src="https://sonycslparis.github.io/diffariff-nime-companion/diags/loop.png" alt="Loop Sampling"/>

By repeating a portion of the data being denoised, we can enforce repetitions in the generated material. We can enforce this repetition for a fraction $$s_\text{Loop}$$ of the diffusion steps, and let the model denoise normally for the remaining last steps to introduce slight variations.
<table class="tg">
<thead>
  <tr>
    <th class="tg-0pky">Number of Repetitions</th>
    <th class="tg-0pky">$$s_\text{Loop}$$</th>
    <th class="tg-0lax" colspan="3">Generations</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-0pky" rowspan="3"> 2 </td>
    <td class="tg-0pky">0.5</td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/loops/0.5_2_reps.mp3"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/loops/0.5_2_reps_1.mp3"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/loops/0.5_2_reps_2.mp3"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <td class="tg-0pky">0.8</td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/loops/0.8_2_reps.mp3"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/loops/0.8_2_reps_1.mp3"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/loops/0.8_2_reps_2.mp3"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <td class="tg-0pky">1</td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/loops/1_2_reps.mp3"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/loops/1_2_reps_1.mp3"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/loops/1_2_reps_2.mp3"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <td class="tg-0pky" rowspan="3"> 4 </td>
    <td class="tg-0pky">0.5</td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/loops/0.5_4_reps.mp3"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/loops/0.5_4_reps_1.mp3"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/loops/0.5_4_reps_2.mp3"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <td class="tg-0pky">0.8</td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/loops/0.8_4_reps.mp3"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/loops/0.8_4_reps_1.mp3"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/loops/0.8_4_reps_2.mp3"> Your Browser does not support the audio tag </audio> </td>
  </tr>
  <tr>
    <td class="tg-0pky">1</td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/loops/1_4_reps.mp3"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/loops/1_4_reps_1.mp3"> Your Browser does not support the audio tag </audio> </td>
    <td class="tg-0lax"> <audio controls preload="none" controlsList="nodownload"><source src="https://sonycslparis.github.io/diffariff-nime-companion/audios/bonus/loops/1_4_reps_2.mp3"> Your Browser does not support the audio tag </audio> </td>
  </tr>
</tbody>
</table>


## Ethics Statement

Sony Computer Science Laboratories is committed to exploring the positive applications of AI in music creation. We collaborate with artists to develop innovative technologies that enhance creativity. We uphold strong ethical standards and actively engage with the music community and industry to align our practices with societal values. Our team is mindful of the extensive work that songwriters and recording artists dedicate to their craft. Our technology must respect, protect, and honour this commitment.

Diff-A-Riff supports and enhances human creativity and emphasises the artist's agency by providing various controls for generating and manipulating musical material. By generating a stem at a time, the artist remains responsible for the entire musical arrangement.

Diff-A-Riff has been trained on a dataset that was legally acquired for internal research and development; therefore, neither the data nor the model can be made publicly available. We are doing our best to ensure full legal compliance and address all ethical concerns.

For media, institutional, or industrial inquiries, please contact us via the email address provided in the paper.

## Extra References
[1] *Barry, Dan, et al. "Go Listen: An End-to-End Online Listening Test Platform." Journal of Open Research Software 9.1 (2021)*

[2] *Liutkus, Antoine, et al. "The 2016 signal separation evaluation campaign." Latent Variable Analysis and Signal Separation: 13th International Conference, LVA/ICA 2017, Grenoble, France, February 21-23, 2017, Proceedings 13. Springer International Publishing, 2017.*


<script>
// upon play of one audio element stop others
for (audio of document.getElementsByTagName("audio")) {
    audio.addEventListener("play", function(event) {
	for (other of document.getElementsByTagName("audio")) {
	    if ( this != other ) {
		other.pause();
    other.currentTime = 0;
	    }
	}
    });
}
</script>
