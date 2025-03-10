<p align="center">
    <img src="./assets/logo/白底.png" width="400" />
</p>

<p align="center">
    <a href="https://map-yue.github.io/">Demo 🎶</a> &nbsp;|&nbsp; 📑 <a href="">Paper (coming soon)</a>
    <br>
    <a href="https://huggingface.co/m-a-p/YuE-s1-7B-anneal-en-cot">YuE-s1-7B-anneal-en-cot 🤗</a> &nbsp;|&nbsp; <a href="https://huggingface.co/m-a-p/YuE-s1-7B-anneal-en-icl">YuE-s1-7B-anneal-en-icl 🤗</a> &nbsp;|&nbsp; <a href="https://huggingface.co/m-a-p/YuE-s1-7B-anneal-jp-kr-cot">YuE-s1-7B-anneal-jp-kr-cot 🤗</a>
    <br>
    <a href="https://huggingface.co/m-a-p/YuE-s1-7B-anneal-jp-kr-icl">YuE-s1-7B-anneal-jp-kr-icl 🤗</a> &nbsp;|&nbsp; <a href="https://huggingface.co/m-a-p/YuE-s1-7B-anneal-zh-cot">YuE-s1-7B-anneal-zh-cot 🤗</a> &nbsp;|&nbsp; <a href="https://huggingface.co/m-a-p/YuE-s1-7B-anneal-zh-icl">YuE-s1-7B-anneal-zh-icl 🤗</a>
    <br>
    <a href="https://huggingface.co/m-a-p/YuE-s2-1B-general">YuE-s2-1B-general 🤗</a> &nbsp;|&nbsp; <a href="https://huggingface.co/m-a-p/YuE-upsampler">YuE-upsampler 🤗</a>
</p>

---
Our model's name is **YuE (乐)**. In Chinese, the word means "music" and "happiness." Some of you may find words that start with Yu hard to pronounce. If so, you can just call it "yeah." We wrote a song with our model's name, see [here](assets/logo/yue.mp3).

YuE is a groundbreaking series of open-source foundation models designed for music generation, specifically for transforming lyrics into full songs (lyrics2song). It can generate a complete song, lasting several minutes, that includes both a catchy vocal track and accompaniment track. YuE is capable of modeling diverse genres/languages/vocal techniques. Please visit the [**Demo Page**](https://map-yue.github.io/) for amazing vocal performance.

## YuE GP for the GPU Poor by DeepBeepMeep

Please first follow the instructions to install the app below.

### YuE versions

There are two versions of the YuE GP which each will download a different huggingspace model:

- Lyrics + Genre prompts (default) : the song will be generated based on the Lyrics and a genre's description
```bash
cd inference
python gradio_server.py
```

- In Context Learning (default), you can provide also audio prompts (either a mixed audio prompt or a vocal and instrumental prompt) to describe your expectations.
```bash
cd inference
python gradio_server.py --icl
```

### Performance profiles
You have access to numerous performance profiles depending on the performance of your GPU:\

To run the Gradio app with profile 1 (default profile, the fastest but requires 16 GB of VRAM):
```bash
cd inference
python gradio_server.py --profile 1
```

To run the Gradio app with profile 3 (default profile, a bit slower and the model is quantized to 8 bits but requires 12 GB of VRAM):
```bash
cd inference
python gradio_server.py --profile 3
```

To run the Gradio app with less than 10 GB of VRAM  profile 4 (very slow as this will incur sequencial offloading):
```bash
cd inference
python gradio_server.py --profile 4
```

If some reason the system seems to be frozen you may be short in VRAM and your GPU is swapping inefficiently data between the RAM and the VRAM. Something consuming even less VRAM makes it faster, it is why I have added a profile 5 which has the minimum possible VRAM consumption:
```bash
cd inference
python gradio_server.py --profile 5
```


If you have a Linux based system / Windows WSL or  were able to install Triton on Windows, you can also turn on Pytorch compilation with '--compile' for a faster generation.  
```bash
cd inference
python gradio_server.py --profile 4 --compile
```
To install Triton on Windows: https://github.com/woct0rdho/triton-windows/releases/download/v3.1.0-windows.post8/triton-3.1.0-cp310-cp310-win_amd64.whl

Likewise if you were not able to install flash attention on Windows, you can force the application to use sdpa attention instead by using the '--sdpa' switch. Be aware that this may requires more VRAM
```bash
cd inference
python gradio_server.py --profile 4 --sdpa
```

You can try a new experimental turbo stage 2 with profile 1 (16 GB+ RAM) that makes stage two times faster. However it is not clear whether this has some impact on the quality of the generated song:
```bash
cd inference
python gradio_server.py --profile 1 --turbo-stage2
```

You may check the mmgp git homepage  (https://github.com/deepbeepmeep/mmgp)  if you want to design your own profiles.

### Other applications for the GPU Poors
If you enjoy this application, you will certainly appreciate these ones too:
- Hunyuan3D-2GP: https://github.com/deepbeepmeep/Hunyuan3D-2GP :\
A great image to 3D or text to 3D tool by the Tencent team. Thanks to mmgp it can run with less than 6 GB of VRAM

- HuanyuanVideoGP: https://github.com/deepbeepmeep/HunyuanVideoGP :\
One of the best open source Text to Video generator

- FluxFillGP: https://github.com/deepbeepmeep/FluxFillGP :\
One of the best inpainting / outpainting tools based on Flux that can run with less than 12 GB of VRAM.

- Cosmos1GP: https://github.com/deepbeepmeep/Cosmos1GP :\
This application include two models: a text to world generator and a image / video to world (probably the best open source image to video generator).

- OminiControlGP: https://github.com/deepbeepmeep/OminiControlGP :\
A flux derived image generator that will allow you to transfer an object of your choosing in a prompted scene. It is optimized to run with ony 6 GB of VRAM.

## News and Updates
* **2025.02.10 🔥**: V3.0 DeepBeepMeep: Added possibility to generate multiple songs per Genres prompt and to generate multiple Genres songs in a row based on the same lyrics. Added also a progression bar and an Abort button. You will need to update the transformers patch for the progression bar to work. I have also added an experimental turbo stage 2 that makes this stage two times faster (use the --turbo-stage2 switch). It will work with 16GB+ VRAM and may produce lesser quality songs.
* **2025.02.08 🔥**: V2.21 DeepBeepMeep: Thanks to olilanz for aligning infer.py with gradio server.py and addding code to reinforce robustness 
* **2025.02.06 🔥**: V2.2 DeepBeepMeep: forgot to remove test code that was slowing down profile 1 and 3
* **2025.02.06 🔥**: V2.1 DeepBeepMeep: 3 times faster with 12+ GB VRAM GPUs (requires Flash Attention 2) thanks to a new optimized transformers libary. You will need to reapply the patchtransformers.sh. Generating a 1 min song takes now only 4 minutes on a RTX 4090 ! Added also progression info in terminal to provide feedback (pending real progression bars).

* **2025.01.30 🔥**: V1.3 DeepBeepMeep: Added support for In Context Learning, now you can provide audio samples prompts to drive the song generation.
* **2025.01.30 🔥**: V1.2 DeepBeepMeep: Speed improvements for low VRAM profiles + patch for transformers library.
* **2025.01.29 🔥**: V1.1 DeepBeepMeep: GPU Poor version.
* **2025.01.26 🔥**: V1.0 We have released the **YuE** series.

<br>

# Installation instructions


Python 3.10 is recommended as some issues have been reported on python 3.12 and 3.13. Python 3.11 might work as well. 

##  1) Install source code
Make sure you have git-lfs installed (https://git-lfs.com)

```
git lfs install
git clone https://github.com/deepbeepmeep/YuEGP/

cd YuEGP/inference/
git clone https://huggingface.co/m-a-p/xcodec_mini_infer
```

## 2) Install torch and requirements
Create a Venv or use Conda and Install torch 2.5.1 with Cuda 12.4 :
```
pip install torch==2.5.1 torchvision torchaudio --index-url https://download.pytorch.org/whl/test/cu124
```

Alternatively if you have an AMD GPU please do the following (many thanks to Hackey for sharing these install instructions): 
```
pip3 install torch torchaudio triton --index-url https://download.pytorch.org/whl/rocm6.2
TORCH_ROCM_AOTRITON_ENABLE_EXPERIMENTAL=1 python gradio_server.py --profile 1 --sdpa
```


Then install dependencies with the following command:

### **GPU Memory**
YuE requires significant GPU memory for generating long sequences. Below are the recommended configurations:
- **For GPUs with 24GB memory or less**: Run **up to 2 sessions** to avoid out-of-memory (OOM) errors. Thanks to the community, there are [YuE-exllamav2](https://github.com/sgsdxzy/YuE-exllamav2) and [YuEGP](https://github.com/deepbeepmeep/YuEGP) for those with limited GPU resources. While both enhance generation speed and coherence, they may compromise musicality. (P.S. Better prompts & ICL help!)
- **For full song generation** (many sessions, e.g., 4 or more): Use **GPUs with at least 80GB memory**. i.e. H800, A100, or multiple RTX4090s with tensor parallel.

## 3) (optional) Install FlashAttention
For saving GPU memory, **FlashAttention 2 is recommended**. Without it, large sequence lengths will lead to out-of-memory (OOM) errors, especially on GPUs with limited memory. Install it using the following command:
```
pip install flash-attn --no-build-isolation
```

Before installing FlashAttention, ensure that your CUDA environment is correctly set up. 
For example, if you are using CUDA 12.4:
- If using a module system:
``` module load cuda12.4/toolkit/12.4.0 ```
- Or manually configure CUDA in your shell:

```
    export PATH=/usr/local/cuda-12.4/bin:$PATH
    export LD_LIBRARY_PATH=/usr/local/cuda-12.4/lib64:$LD_LIBRARY_PATH
```


**As an alternative if you were unable to install Flash attention (usually a pain on Windows) you can use sdpa attention instead by adding the *--sdpa* switch when running the gradio server. However this may consume more VRAM.**


## 4) (optional) Transformers Patches for Low VRAM (< 10 GB of VRAM) and 2x faster genration with more than 16 GB of VRAM
If you have no choice but to use a low VRAM profile (profile 4 or profile 5), I am providing a patch for the transformers libray that should double the speed of the transformers libary (note this patch offers little little improvements on other profiles), this patch overwrites two files from the transformers libary. You can either copy and paste my 'transformers' folder in your venv or run the script below if the venv directory is just below the app directory:

Update: I have added another patch which double the speed of stage 2 of the generation process for all profiles and also triple the speed of stage 1 for profile 1 and 3 (16 GB VRAM +). You will need to install Flash Attention 2 for this second patch to work.

For Linux:
```
source patchtransformers.sh
```

For Windows:
```
patchtransformers.bat
```
## GPU Memory Usage and Sessions

Without the optimizations, YuE requires significant GPU memory for generating long sequences. 

If you have out of memory errors while a lot memory still seems to be free,  please try the following before lauching the app (many thanks to olilanz for this finding)  :  
```
export PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True
```


Below are the recommended configurations:

- **For GPUs with 24GB memory or less**: Run **up to 2 sessions** concurrently to avoid out-of-memory (OOM) errors.
- **For full song generation** (many sessions, e.g., 4 or more): Use **GPUs with at least 80GB memory**. This can be achieved by combining multiple GPUs and enabling tensor parallelism.

To customize the number of sessions, the interface allows you to specify the desired session count. By default, the model runs **2 sessions** for optimal memory usage.

---


### Running the Script
Here’s a quick guide to help you generate music with **YuE** using 🤗 Transformers. Before running the code, make sure your environment is properly set up, and that all dependencies are installed.
In the following example, customize the `genres` and `lyrics` in the script, then execute it to generate a song with **YuE**.

```bash
# This is the CoT mode.
cd YuE/inference/
python infer.py \
    --cuda_idx 0 \
    --stage1_model m-a-p/YuE-s1-7B-anneal-en-cot \
    --stage2_model m-a-p/YuE-s2-1B-general \
    --genre_txt ../prompt_egs/genre.txt \
    --lyrics_txt ../prompt_egs/lyrics.txt \
    --run_n_segments 2 \
    --stage2_batch_size 4 \
    --output_dir ../output \
    --max_new_tokens 3000 \
    --repetition_penalty 1.1
```

We also support music in-context-learning (provide a reference song), there are 2 types: single-track (mix/vocal/instrumental) and dual-track. 

Note: 
- ICL requires a different ckpt, e.g. `m-a-p/YuE-s1-7B-anneal-en-icl`.

- Music ICL generally requires a 30s audio segment. The model will write new songs with similar style of the provided audio, and may improve musicality.

- Dual-track ICL works better in general, requiring both vocal and instrumental tracks.

- For single-track ICL, you can provide a mix, vocal, or instrumental track.

- You can separate the vocal and instrumental tracks using [python-audio-separator](https://github.com/nomadkaraoke/python-audio-separator) or [Ultimate Vocal Remover GUI](https://github.com/Anjok07/ultimatevocalremovergui).

```bash
# This is the dual-track ICL mode.
# To turn on dual-track mode, enable `--use_dual_tracks_prompt`
# and provide `--vocal_track_prompt_path`, `--instrumental_track_prompt_path`, 
# `--prompt_start_time`, and `--prompt_end_time`
# The ref audio is taken from GTZAN test set.
cd YuE/inference/
python infer.py \
    --cuda_idx 0 \
    --stage1_model m-a-p/YuE-s1-7B-anneal-en-icl \
    --stage2_model m-a-p/YuE-s2-1B-general \
    --genre_txt ../prompt_egs/genre.txt \
    --lyrics_txt ../prompt_egs/lyrics.txt \
    --run_n_segments 2 \
    --stage2_batch_size 4 \
    --output_dir ../output \
    --max_new_tokens 3000 \
    --repetition_penalty 1.1 \
    --use_dual_tracks_prompt \
    --vocal_track_prompt_path ../prompt_egs/pop.00001.Vocals.mp3 \
    --instrumental_track_prompt_path ../prompt_egs/pop.00001.Instrumental.mp3 \
    --prompt_start_time 0 \
    --prompt_end_time 30 
```

```bash
# This is the single-track (mix/vocal/instrumental) ICL mode.
# To turn on single-track ICL, enable `--use_audio_prompt`, 
# and provide `--audio_prompt_path` , `--prompt_start_time`, and `--prompt_end_time`. 
# The ref audio is taken from GTZAN test set.
cd YuE/inference/
python infer.py \
    --cuda_idx 0 \
    --stage1_model m-a-p/YuE-s1-7B-anneal-en-icl \
    --stage2_model m-a-p/YuE-s2-1B-general \
    --genre_txt ../prompt_egs/genre.txt \
    --lyrics_txt ../prompt_egs/lyrics.txt \
    --run_n_segments 2 \
    --stage2_batch_size 4 \
    --output_dir ../output \
    --max_new_tokens 3000 \
    --repetition_penalty 1.1 \
    --use_audio_prompt \
    --audio_prompt_path ../prompt_egs/pop.00001.mp3 \
    --prompt_start_time 0 \
    --prompt_end_time 30 
```
---
 
## Prompt Engineering Guide
The prompt consists of three parts: genre tags, lyrics, and ref audio.

### Genre Tagging Prompt
1. An example genre tagging prompt can be found [here](prompt_egs/genre.txt).

2. A stable tagging prompt usually consists of five components: genre, instrument, mood, gender, and timbre. All five should be included if possible, separated by space (space delimiter).

3. Although our tags have an open vocabulary, we have provided the top 200 most commonly used [tags](./top_200_tags.json). It is recommended to select tags from this list for more stable results.

3. The order of the tags is flexible. For example, a stable genre tagging prompt might look like: "inspiring female uplifting pop airy vocal electronic bright vocal vocal."

4. Additionally, we have introduced the "Mandarin" and "Cantonese" tags to distinguish between Mandarin and Cantonese, as their lyrics often share similarities.

### Lyrics Prompt
1. An example lyric prompt can be found [here](prompt_egs/lyrics.txt).

2. We support multiple languages, including but not limited to English, Mandarin Chinese, Cantonese, Japanese, and Korean. The default top language distribution during the annealing phase is revealed in [issue 12](https://github.com/multimodal-art-projection/YuE/issues/12#issuecomment-2620845772). A language ID on a specific annealing checkpoint indicates that we have adjusted the mixing ratio to enhance support for that language.

3. The lyrics prompt should be divided into sessions, with structure labels (e.g., [verse], [chorus], [bridge], [outro]) prepended. Each session should be separated by 2 newline character "\n\n".

4. **DONOT** put too many words in a single segment, since each session is around 30s (`--max_new_tokens 3000` by default).

5. We find that [intro] label is less stable, so we recommend starting with [verse] or [chorus].

6. For generating music with no vocal (instrumental only), see [issue 18](https://github.com/multimodal-art-projection/YuE/issues/18).


### Audio Prompt

1. Audio prompt is optional. Providing ref audio for ICL usually increase the good case rate, and result in less diversity since the generated token space is bounded by the ref audio. CoT only (no ref) will result in a more diverse output.

2. We find that dual-track ICL mode gives the best musicality and prompt following. 

3. Use the chorus part of the music as prompt will result in better musicality.

4. Around 30s audio is recommended for ICL.

5. For music continuation, see [YuE-extend by Mozer](https://github.com/Mozer/YuE-extend). Also supports Colab.

---

## License Agreement \& Disclaimer  
- The YuE model (including its weights) is now released under the **Apache License, Version 2.0**. We do not make any profit from this model, and we hope it can be used for the betterment of human creativity.
- **Use & Attribution**: 
    - We encourage artists and content creators to freely incorporate outputs generated by YuE into their own works, including commercial projects. 
    - We encourage attribution to the model’s name (“YuE by HKUST/M-A-P”), especially for public and commercial use. 
- **Originality & Plagiarism**: It is the sole responsibility of creators to ensure that their works, derived from or inspired by YuE outputs, do not plagiarize or unlawfully reproduce existing material. We strongly urge users to perform their own due diligence to avoid copyright infringement or other legal violations.
- **Recommended Labeling**: When uploading works to streaming platforms or sharing them publicly, we **recommend** labeling them with terms such as: “AI-generated”, “YuE-generated", “AI-assisted” or “AI-auxiliated”. This helps maintain transparency about the creative process.
- **Disclaimer of Liability**: 
    - We do not assume any responsibility for the misuse of this model, including (but not limited to) illegal, malicious, or unethical activities. 
    - Users are solely responsible for any content generated using the YuE model and for any consequences arising from its use. 
    - By using this model, you agree that you understand and comply with all applicable laws and regulations regarding your generated content.

---

## Acknowledgements
The project is co-lead by HKUST and M-A-P (alphabetic order). Also thanks moonshot.ai, bytedance, 01.ai, and geely for supporting the project.
A friendly link to HKUST Audio group's [huggingface space](https://huggingface.co/HKUSTAudio). 

We deeply appreciate all the support we received along the way. Long live open-source AI!

---

## Citation

If you find our paper and code useful in your research, please consider giving a star :star: and citation :pencil: :)

```BibTeX
@misc{yuan2025yue,
  title={YuE: Open Music Foundation Models for Full-Song Generation},
  author={Ruibin Yuan and Hanfeng Lin and Shawn Guo and Ge Zhang and Jiahao Pan and Yongyi Zang and Haohe Liu and Xingjian Du and Xeron Du and Zhen Ye and Tianyu Zheng and Yinghao Ma and Minghao Liu and Lijun Yu and Zeyue Tian and Ziya Zhou and Liumeng Xue and Xingwei Qu and Yizhi Li and Tianhao Shen and Ziyang Ma and Shangda Wu and Jun Zhan and Chunhui Wang and Yatian Wang and Xiaohuan Zhou and Xiaowei Chi and Xinyue Zhang and Zhenzhu Yang and Yiming Liang and Xiangzhou Wang and Shansong Liu and Lingrui Mei and Peng Li and Yong Chen and Chenghua Lin and Xie Chen and Gus Xia and Zhaoxiang Zhang and Chao Zhang and Wenhu Chen and Xinyu Zhou and Xipeng Qiu and Roger Dannenberg and Jiaheng Liu and Jian Yang and Stephen Huang and Wei Xue and Xu Tan and Yike Guo}, 
  howpublished={\url{https://github.com/multimodal-art-projection/YuE}},
  year={2025},
  note={GitHub repository}
}
```
<br>
