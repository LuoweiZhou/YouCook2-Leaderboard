# YouCook2-Leaderboard
This repo hosts leaderboards on YouCook2 and related resources. The main tasks concerned include text-to-video retrieval, video captioning, dense video description (captioning), and object grounding. Results are on the validation set unless otherwise specified. Note that until now, leaderboard on the test set is only available for the dense video description and object groudning (see [FQA](#faq) for details). See also our [policy](#faq) on arXiv papers.

**To have your model added to the leaderboard, please reach out to [us](mailto:luozhou@microsoft.com) or submit a PR..**


## Text-to-Video Retrieval
"Video" here refers to video **clips** (a cooking video usually contains multiple steps/clips). Each clip is considered an individual video assuming the clip boundary is given. Text to **whole** video retrieval is not considered in this task.

| Methods      |  R@1 (▲) |  R@5 (▲) | R@10 (▲) | Median R (▼) | Proceedings | Link to Paper | Notes |
|:------------:|---------:|---------:|---------:|-------------:|:-----------:|:-------------:|:-----:|
| Random       |    0.0   |    0.2   |    0.3   |       1675   |             |               | reported in [Miech et al., 2019](https://openaccess.thecvf.com/content_ICCV_2019/papers/Miech_HowTo100M_Learning_a_Text-Video_Embedding_by_Watching_Hundred_Million_Narrated_ICCV_2019_paper.pdf) |
| HGLMM        |    4.6   |   14.3   |   21.6   |         75   | CVPR 2015   | [Klein et al.](https://www.cv-foundation.org/openaccess/content_cvpr_2015/papers/Klein_Associating_Neural_Word_2015_CVPR_paper.pdf) | zero-shot; reported in [Miech et al., 2019](https://openaccess.thecvf.com/content_ICCV_2019/papers/Miech_HowTo100M_Learning_a_Text-Video_Embedding_by_Watching_Hundred_Million_Narrated_ICCV_2019_paper.pdf) |
| Miech et al. |    6.1   |   17.3   |   24.8   |         46   | ICCV 2019   | [Miech et al.](https://openaccess.thecvf.com/content_ICCV_2019/papers/Miech_HowTo100M_Learning_a_Text-Video_Embedding_by_Watching_Hundred_Million_Narrated_ICCV_2019_paper.pdf) | zero-shot |
| ActBERT      |    9.6   |   26.7   |   38.0   |         19   | CVPR 2020   | [Zhu et al.](https://openaccess.thecvf.com/content_CVPR_2020/papers/Zhu_ActBERT_Learning_Global-Local_Video-Text_Representations_CVPR_2020_paper.pdf) | zero-shot
| MIL-NCE      | **13.9** | **36.3** | **48.9** |       **11** | CVPR 2020   | [Miech et al.](https://openaccess.thecvf.com/content_CVPR_2020/papers/Miech_End-to-End_Learning_of_Visual_Representations_From_Uncurated_Instructional_Videos_CVPR_2020_paper.pdf) | zero-shot; rerun with the official [code](https://github.com/antoine77340/MIL-NCE_HowTo100M) |
| Miech et al. |  **8.2** | **24.5** | **35.3** |       **24** | ICCV 2019   | [Miech et al.](https://openaccess.thecvf.com/content_ICCV_2019/papers/Miech_HowTo100M_Learning_a_Text-Video_Embedding_by_Watching_Hundred_Million_Narrated_ICCV_2019_paper.pdf) | |

All these results were evaluated using [a subset of 3,350/3,492 video clips from the validation set that aren't present in HowTo100M](https://github.com/antoine77340/MIL-NCE_HowTo100M/blob/master/csv/validation_youcook.csv).


## Video Captioning
In this task, we assume GT event segments are available (i.e., the start and end timestamps of each recipe step).

Evaluation on this task diverges into two modes: micro-level and macro-level. In micro-level evaluation, the averaged score on all segments is reported. In macro-level evaluation, language metric scores are first averaged across segments within each video and then averaged across the dataset.

- Micro-level

| Methods      |Input Modality|   B@3  |   B@4  |   M   |    C   | Proceedings |Link to Paper| Notes |
|:------------:|:---------:|-------:|-------:|-------:|-------:|:-----------:|:-------------:|:-----:|
| ActBERT | V | 8.66 | 5.41 | 13.30 | 65.0 | CVPR 2020 | [Zhu et al.](https://openaccess.thecvf.com/content_CVPR_2020/papers/Zhu_ActBERT_Learning_Global-Local_Video-Text_Representations_CVPR_2020_paper.pdf) |  |
| VideoBERT | V | 7.59 | 4.33 | 11.94 | 55.0 | ICCV 2019 | [Sun et al.](https://openaccess.thecvf.com/content_ICCV_2019/papers/Sun_VideoBERT_A_Joint_Model_for_Video_and_Language_Representation_Learning_ICCV_2019_paper.pdf) |  |
| Masked Trans. | V | 7.53 | 3.85 | 10.68\* | 37.9 | CVPR 2018 | [Zhou et al.](https://openaccess.thecvf.com/content_cvpr_2018/papers/Zhou_End-to-End_Dense_Video_CVPR_2018_paper.pdf) | \*Erratum on the original [VideoBERT](https://openaccess.thecvf.com/content_ICCV_2019/papers/Sun_VideoBERT_A_Joint_Model_for_Video_and_Language_Representation_Learning_ICCV_2019_paper.pdf) report |
| AT | T | - | 8.55 | 16.93 | 106.0 | CoNLL 2019 | [Hessel et al.](https://arxiv.org/pdf/1910.02930.pdf) |  |
| AT+Video | V+T | - | 9.01 | 17.77 | 112.0 | CoNLL 2019 | [Hessel et al.](https://arxiv.org/pdf/1910.02930.pdf) |  |
| DPC | V+T | 7.60 | 2.76 | 18.08 | - | ACL 2019 | [Shi et al.](https://www.aclweb.org/anthology/P19-1641.pdf) |  |

- Macro-level

| Methods      |Input Modality|   B@3  |   B@4  |   M   |    C   | Proceedings |Link to Paper| Notes |
|:------------:|:---------:|-------:|-------:|-------:|-------:|:-----------:|:-------------:|:-----:|
| Masked Trans. | V | 5.08 | 1.42 | 11.20 | 45.13 | CVPR 2018 | [Zhou et al.](https://openaccess.thecvf.com/content_cvpr_2018/papers/Zhou_End-to-End_Dense_Video_CVPR_2018_paper.pdf) |  |


Note: `B@3` - `BLEU@3`, `B@4` - `BLEU@4`, `M` - `METEOR`, and `C` - `CIDEr`. The input modalities include video (V), transcript (T) or both (V+T). Evaluation code: [Macro-level](https://github.com/LuoweiZhou/densevid_eval_spice) and [Micro-level](https://github.com/tylin/coco-caption).


## Dense Video Description
In this task, we need to predict both event segments and description. The [latest](https://github.com/ranjaykrishna/densevid_eval) evaluation tool from ActivityNet Captions challenge is adopted (vs. the 2017 buggy [version](https://github.com/ranjaykrishna/densevid_eval/tree/b8d90707984bf9c99454ba82b089006f14fb62b3)).

| Methods      |   B@3  |   B@4  |   M   |    C   | Proceedings |Link to Paper| Notes |
|:------------:|-------:|-------:|-------:|-------:|:-----------:|:-----------:|:-----:|
| Masked Transformer | 0.72 | 0.30 | 3.18 | 6.10 | CVPR 2018 | [Zhou et al.](https://openaccess.thecvf.com/content_cvpr_2018/papers/Zhou_End-to-End_Dense_Video_CVPR_2018_paper.pdf) |  |

A dedicated evaluation server on the **test set** (to be updated to the latest eval code soon): https://competitions.codalab.org/competitions/20594


## Object Grounding

| Methods | Box Acc. (Val) | Box Acc. (Test) | Proceedings | Link to Paper | Notes |
|:------------:|----------:|----------:|:-----------:|:-----------:|:-----:|
| DVSA + (lw, obj int) | 30.31 | 31.73 | BMVC 2018 | [Zhou et al.](https://arxiv.org/pdf/1805.02834.pdf) |  |

A dedicated evaluation server on this task (including on the **test set**): https://competitions.codalab.org/competitions/20302


## <a name='faq'></a> FAQ
- Why there is no test set?

We withheld the test split of YouCook2 for evaluation purposes on event proposal generation and dense video description. Releasing the event segments (for video captioning) or captions (for text-to-video retrieval) break the confidentiality of the leaderboard on the former two tasks. However, since there is a growing interest in the latter two tasks, we are considering releasing the full test annotation for research purposes. This will likely happen after the CVPR'21 deadline.

- Can I submit results from arXiv papers?

For now, we only accept **published** works to maintain a high result credibility. Therefore, we do not accept results from arXiv papers.


## Reference (bibtex)
YouCook2 dataset paper:
```bibtex
@inproceedings{ZhXuCoCVPR18,
    author={Zhou, Luowei and Xu, Chenliang and Corso, Jason J},
    title = {Towards Automatic Learning of Procedures From Web Instructional Videos},
    booktitle = {AAAI Conference on Artificial Intelligence},
    pages={7590--7598},
    year = {2018},
    url = {https://www.aaai.org/ocs/index.php/AAAI/AAAI18/paper/view/17344}
}
```

Video description benchmark on YouCook2:
```bibtex
@inproceedings{zhou2018end,
    title={End-to-End Dense Video Captioning with Masked Transformer},
    author={Zhou, Luowei and Zhou, Yingbo and Corso, Jason J and Socher, Richard and Xiong, Caiming},
    booktitle={Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition},
    pages={8739--8748},
    year={2018}
}
```

YouCook2-BoundingBox:
```bibtex
@inproceedings{ZhLoCoBMVC18,
    author={Zhou, Luowei and Louis, Nathan and Corso, Jason J},
    title={Weakly-Supervised Video Object Grounding from Text by Loss Weighting and Object Interaction},
    booktitle = {British Machine Vision Conference},
    year = {2018},
    url = {http://bmvc2018.org/contents/papers/0070.pdf}
}
```


## Other resources
A list of leaderboards from Paperwithcode (results might be incomplete):

https://paperswithcode.com/sota/video-retrieval-on-youcook2

https://paperswithcode.com/sota/video-captioning-on-youcook2

VideoPentathlon challenge: https://www.robots.ox.ac.uk/~vgg/challenges/video-pentathlon/challenge.html

VideoBERT and CBT:

https://openaccess.thecvf.com/content_ICCV_2019/papers/Sun_VideoBERT_A_Joint_Model_for_Video_and_Language_Representation_Learning_ICCV_2019_paper.pdf

https://arxiv.org/pdf/1906.05743.pdf

HowTo100M / MIL-NCE:

https://www.di.ens.fr/willow/research/howto100m/

https://openaccess.thecvf.com/content_CVPR_2020/papers/Miech_End-to-End_Learning_of_Visual_Representations_From_Uncurated_Instructional_Videos_CVPR_2020_paper.pdf

MSRVTT: https://www.microsoft.com/en-us/research/publication/msr-vtt-a-large-video-description-dataset-for-bridging-video-and-language/

Epic-Kitchen: https://epic-kitchens.github.io/2020-100

SOTA Dense Video Captioning metric: https://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123510511.pdf


## Acknowledgement
Thanks to Santiago for suggesting and contributing to the leaderboard. Thanks for Nathan for contributing to the Object Grounding part.
