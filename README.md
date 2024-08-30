# Universal echocardiography interpretation with multi-task deep learning

[**Gregory Holste**](https://gholste.me), Evangelos K. Oikonomou, Zhangyang Wang, [**Rohan Khera**](https://www.cards-lab.org/current)

-----

## PanEcho

<p align=center>
    <img src=content/panecho.png height=500>
</p>

**Overview of PanEcho.** **(a)** Schematic of PanEcho, a view-agnostic, multi-task deep learning model that automatically performs 39 transthoracic echocardiography interpretation tasks from echocardiogram videos. PanEcho consists of an image encoder to embed individual video frames, a temporal Transformer to learn temporal associations over frames in a video, and task-specific output heads to perform a wide variety of classification and regression tasks. The model was trained end-to-end on over one million echocardiograms from YNHHS hospitals with a multi-task learning objective. **(b)** Since PanEcho is view-agnostic, multi-view echocardiography can be leveraged to integrate information across views. At test time, predictions are aggregated across echocardiograms acquired during the same study to generate study-level predictions for each task. For all 39 tasks, PanEcho was validated internally on temporally held-out data from YNHHS hospitals. For tasks with publicly available labels, PanEcho was also externally validated on EchoNet-LVH and EchoNet-Dynamic, two large-scale single-view echocardiography datasets for assessing LV structure and function.

## Contact

Reach out to Greg Holste (gholste@utexas.edu) and Rohan Khera (rohan.khera@yale.edu) with any questions!
