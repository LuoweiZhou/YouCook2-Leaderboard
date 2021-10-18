# YouCook2-Leaderboard

**(Updated on 02/03/2021) We have released the test set (segments only) to support the video captioning and object grounding task. ([Link to Download](http://youcook2.eecs.umich.edu/download)) In addition, we will release a set of caption annotations / evaluation server for the retrieval task separately later this year.**

This repo hosts leaderboards on [YouCook2 dataset](http://youcook2.eecs.umich.edu/) and related resources. The main tasks concerned include text-to-video retrieval, video captioning, dense video description (captioning), and object grounding. Results are on the validation set unless otherwise specified. Note that until now, evaluation servers on the test set are available for video captioning, dense video description and object groudning (see [FQA](#faq) for details). We will add the support on video retrieval soon. See also our [policy](#faq) on arXiv papers.

**To have your model added to the leaderboard, please reach out to [us](mailto:luozhou@microsoft.com) or submit a PR.**


## Text-to-Video Retrieval
"Video" here refers to video **clips** (a cooking video usually contains multiple steps/clips). Each clip is considered an individual video assuming the clip boundary is given. Text to **whole** video retrieval is not considered in this task.

| Methods      |  R@1 (▲) |  R@5 (▲) | R@10 (▲) | Median R (▼) | Proceedings | Link to Paper | Notes |
|:------------:|---------:|---------:|---------:|-------------:|:-----------:|:-------------:|:-----:|
| Random       |    0.0   |    0.2   |    0.3   |       1675   |             |               | reported in [Miech et al., 2019](https://openaccess.thecvf.com/content_ICCV_2019/papers/Miech_HowTo100M_Learning_a_Text-Video_Embedding_by_Watching_Hundred_Million_Narrated_ICCV_2019_paper.pdf) |
| HGLMM        |    4.6   |   14.3   |   21.6   |         75   | CVPR 2015   | [Klein et al.](https://www.cv-foundation.org/openaccess/content_cvpr_2015/papers/Klein_Associating_Neural_Word_2015_CVPR_paper.pdf) | zero-shot; reported in [Miech et al., 2019](https://openaccess.thecvf.com/content_ICCV_2019/papers/Miech_HowTo100M_Learning_a_Text-Video_Embedding_by_Watching_Hundred_Million_Narrated_ICCV_2019_paper.pdf) |
| Miech et al. |    6.1   |   17.3   |   24.8   |         46   | ICCV 2019   | [Miech et al.](https://openaccess.thecvf.com/content_ICCV_2019/papers/Miech_HowTo100M_Learning_a_Text-Video_Embedding_by_Watching_Hundred_Million_Narrated_ICCV_2019_paper.pdf) | zero-shot |
| ActBERT      |    9.6   |   26.7   |   38.0   |         19   | CVPR 2020   | [Zhu et al.](https://openaccess.thecvf.com/content_CVPR_2020/papers/Zhu_ActBERT_Learning_Global-Local_Video-Text_Representations_CVPR_2020_paper.pdf) | zero-shot
| MIL-NCE      | 13.9 | 36.3 | 48.9 |       11 | CVPR 2020   | [Miech et al.](https://openaccess.thecvf.com/content_CVPR_2020/papers/Miech_End-to-End_Learning_of_Visual_Representations_From_Uncurated_Instructional_Videos_CVPR_2020_paper.pdf) | zero-shot; rerun with the official [code](https://github.com/antoine77340/MIL-NCE_HowTo100M) |
| MMV      | 11.7 | 33.4 | 45.4 |       13 | NeurIPS 2020   | [Alayrac et al.](https://proceedings.neurips.cc/paper/2020/file/0060ef47b12160b9198302ebdb144dcf-Paper.pdf) | zero-shot; 3190 test video clips |
| MCN      | 18.1 | 35.5 | 45.2 |       - | ICCV 2021   | [Chen et al.](https://rpand002.github.io/data/ICCV_2021_mcn.pdf) | zero-shot  |
| VideoCLIP      | **22.7** | **50.4** | **63.1** |       - | EMNLP 2021   | [Xu et al.](https://arxiv.org/pdf/2109.14084.pdf) | zero-shot; 3305 test video clips |
| VATT      | - | - | 45.5 |       - | NeurIPS 2021   | [Akbari et al.](https://arxiv.org/pdf/2104.11178.pdf) | zero-shot; 3.1K test video clips |
| Miech et al. |  8.2 | 24.5 | 35.3 |       24 | ICCV 2019   | [Miech et al.](https://openaccess.thecvf.com/content_ICCV_2019/papers/Miech_HowTo100M_Learning_a_Text-Video_Embedding_by_Watching_Hundred_Million_Narrated_ICCV_2019_paper.pdf) | |
| COOT |  16.7 | 40.2 | 52.3 | **9.0** | NeurIPS 2020   | [Ging et al.](https://proceedings.neurips.cc//paper/2020/file/ff0abbcc0227c9124a804b084d161a2d-Paper.pdf) | 3.2K test video clips |
| DECEMBERT |  **17.0** | **43.8** | **59.8** | **9.0** | NAACL 2020   | [Tang et al.](https://aclanthology.org/2021.naacl-main.193.pdf) | |

**To the best of our knowledge, results were evaluated using [a subset of 3,350/3,492 video clips from the validation set that aren't present in HowTo100M](https://github.com/antoine77340/MIL-NCE_HowTo100M/blob/master/csv/validation_youcook.csv) unless otherwise noted. We highly recommend using this subset for a fair comparison.**


## Video Captioning
In this task, we assume GT event segments are available (i.e., the start and end timestamps of each recipe step).

Evaluation on this task diverges into three modes: micro-level, macro-level, and paragraph-level. In micro-level evaluation, the averaged score on all segments is reported. In macro-level evaluation, language metric scores are first averaged across segments within each video and then averaged across the dataset. In paragraph-level, all the sgement level captions are firstly concatenated together as a paragraph and evaluated against the GT paragraph, the resulting metric scores are then averaged across all videos.

- Micro-level

| Methods      |Input Modality|   B@3  |   B@4  |   M   |    C   | Proceedings |Link to Paper| Notes |
|:------------:|:---------:|-------:|-------:|-------:|-------:|:-----------:|:----------------:|:----:|
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


- Paragraph-level

| Methods      |Input Modality|  B@4  |   M   |    C   | R@4 | Proceedings |Link to Paper| Notes |
|:------------:|:---------:|-------:|-------:|-------:|-------:|:-----------:|:-------------:|:-----:|
| Masked Trans. | V | 7.62 | 15.65 | 32.26 | 7.83 | CVPR 2018 | [Zhou et al.](https://openaccess.thecvf.com/content_cvpr_2018/papers/Zhou_End-to-End_Dense_Video_CVPR_2018_paper.pdf) | reported by [Lei et al.](https://arxiv.org/pdf/2005.05402.pdf) |
| Trnsformer-XL | V | 6.56 | 14.76 | 26.35 | 6.30 | ACL 2019 | [Dai et al.](https://arxiv.org/pdf/1901.02860.pdf) | adopted and reported by [Lei et al.](https://arxiv.org/pdf/2005.05402.pdf) |
| MART | V | 8.00 | 15.9 | 35.74 | 4.39 |ACL 2020 | [Lei et al.](https://arxiv.org/pdf/2005.05402.pdf) | [code](https://github.com/jayleicn/recurrent-transformer)  |
| COOT+MART | V | 11.30 | 19.85 | 57.24 | 6.69 | NeurIPS 2020 | [Ging et al.](https://proceedings.neurips.cc/paper/2020/file/ff0abbcc0227c9124a804b084d161a2d-Paper.pdf) | MART model with COOT features, [code](https://github.com/gingsi/coot-videotext) |



Note: `B@3` - `BLEU@3`, `B@4` - `BLEU@4`, `M` - `METEOR`, and `C` - `CIDEr`. `R@4` denotes the degree of 4-gram repetitions. The input modalities include video (V), transcript (T) or both (V+T). Evaluation code: [Macro-level](https://github.com/LuoweiZhou/densevid_eval_spice), [Micro-level](https://github.com/tylin/coco-caption), and [Paragraph-level](https://github.com/jayleicn/recurrent-transformer/tree/master/densevid_eval).


## Dense Video Description
In this task, we need to predict both event segments and description. The [latest](https://github.com/ranjaykrishna/densevid_eval) evaluation tool from ActivityNet Captions challenge is adopted (vs. the 2017 buggy [version](https://github.com/ranjaykrishna/densevid_eval/tree/b8d90707984bf9c99454ba82b089006f14fb62b3)). Starter code see [densecap](https://github.com/salesforce/densecap).

| Methods      |   B@3  |   B@4  |   M   |    C   | Proceedings |Link to Paper| Notes |
|:------------:|-------:|-------:|-------:|-------:|:-----------:|:-----------:|:-----:|
| Masked Transformer | 0.72 | 0.30 | 3.18 | 6.10 | CVPR 2018 | [Zhou et al.](https://openaccess.thecvf.com/content_cvpr_2018/papers/Zhou_End-to-End_Dense_Video_CVPR_2018_paper.pdf) |  |

A dedicated evaluation server on the **test set** (to be updated to the latest eval code soon): https://competitions.codalab.org/competitions/20594


## Object Grounding
The goal of this task is to spatial-temporally locate/ground object words (provided by a description) to the associated video clip. Starter code and evaluation protocol see [DVSA](https://github.com/MichiganCOG/Video-Grounding-from-Text).

| Methods | Box Acc. (Val) | Box Acc. (Test) | Proceedings | Link to Paper | Notes |
|:------------:|----------:|----------:|:-----------:|:-----------:|:-----:|
| DVSA + (lw, obj int) | 30.31 | 31.73 | BMVC 2018 | [Zhou et al.](https://arxiv.org/pdf/1805.02834.pdf) |  |

A dedicated evaluation server on this task (including on the **test set**): https://competitions.codalab.org/competitions/20302


## <a name='faq'></a> FAQ
- Why there is no test set?

(Updated on 02/03/2021) We have released the test set (segments only) to support the video captioning and object grounding task. ([Link to Download](http://youcook2.eecs.umich.edu/download)) In addition, we will release a set of caption annotations / evaluation server for the retrieval task separately later this year.

We withheld the test split of YouCook2 for evaluation purposes on event proposal generation and dense video description. Releasing the event segments (for video captioning) or captions (for text-to-video retrieval) break the confidentiality of the leaderboard on the former two tasks.

- Can I submit results from arXiv papers?

For now, we only accept **published** works to maintain a high result credibility. Therefore, we do not accept results from arXiv papers.


## Reference (bibtex)
YouCook2 dataset paper:
```bibtex
@inproceedings{ZhXuCoAAAI18,
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
@inproceedings{ZhZhCoCVPR2018,
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
YouCook2 has been supported in part by Google, ARO W911NF-15-1-0354, NSF CNS 1463102, NSF CNS 1628987, NSF NRI 1522904 and NSF BIGDATA 1741472. This repo solely reflects the opinions and conclusions of its authors and neither Google, ARO nor NSF. Thanks to Santiago for suggesting and contributing to the leaderboard. Thanks for Nathan for contributing to the Object Grounding part.
