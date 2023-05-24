---
layout: default
permalink: /
title: ""
---

![](assets/images/introduction.jpg)

---

### **Overview**

_We present a novel in-situ dataset for animal behavior recognition from drone videos. The dataset is focused on Kenyan wildlife and contains behaviors of giraffes, plains zebras, and Grevy's zebras. The videos curated to form the dataset were collected by flying drones over animals at the Mpala Research Centre in Kenya, in accordance with Research License No. NACOSTI/P/22/18214. The data collection protocol adhered strictly to the guidelines set forth by the Institutional Animal Care and Use Committee under permission No. IACUC 1835F. The dataset consists of more than 10 hours of annotated videos, and it includes eight different classes, encompassing seven types of animal behavior and an additional category for occluded instances. In the annotation process for this dataset, a team of 10 people was involved, with an expert zoologist overseeing the process. Each behavior was labeled based on its distinctive features, using a standardized set of criteria to ensure consistency and accuracy across the annotations.  We believe that this dataset will be a valuable resource for researchers working on animal behavior recognition, as it provides a diverse set of annotated videos that can be used for evaluating deep learning models. Additionally, the dataset can be used to study the behavior patterns of Kenyan animals and can help to inform conservation efforts and wildlife management strategies. We provide a detailed description of the dataset and its annotation process, along with some initial experiments on the dataset using conventional deep learning models. The results demonstrate the effectiveness of the dataset for animal behavior recognition and highlight the potential for further research in this area._

---

### **Mini-Scenes**

![](assets/images/mini-scene.jpg)

Our approach to extracting animal tracks from drone videos is a method that we refer to as **mini-scenes** extraction. This technique involves using object detection and tracking methods to simulate the camera being perfectly aligned with each individual animal in the frame. By using mini-scenes, we are able to compensate for any drone movements and provide a stable, zoomed-in representation of the animal's behavior. This approach allows for accurate tracking of individual animals within a group, making it particularly useful for studying social dynamics among animals.

---

### **Extraction**

To implement our mini-scenes approach, we utilized [YOLOv8](https://github.com/ultralytics/ultralytics) object detection algorithm to detect the animals in each frame and improved [SORT](https://arxiv.org/abs/1602.00763) tracking algorithm to follow their movement. We then extracted a small window around each animal to create the mini-scenes.

<span id="youtube-standard-label">Examples of mini-scenes extraction are available on YouTube:</span>

<span id="youtube-minimized-label">Examples of mini-scenes extraction are available on YouTube: [**`Giraffes`**](https://youtu.be/_whg7JULVb8), [**`Zebras`**](https://youtu.be/msalx6tQ8RM).</span>

<table id="youtube-table">
    <thead>
        <tr>
            <td id="td-g">
                <iframe width="805" height="453" src="https://www.youtube-nocookie.com/embed/_whg7JULVb8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
            </td>
            <td id="td-z">
                <iframe width="805" height="453" src="https://www.youtube-nocookie.com/embed/msalx6tQ8RM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
            </td>
        </tr>
    </thead>
</table>

---

### **Data Collection and Processing**
The drone videos were collected by our team at Mpala Research Centre, Kenya in January 2023. Animal behaviors of giraffes, plains zebras, and Grevy’s zebras were recorded using DJI Mavic 2S drones in 5.4K resolution.

We developed [**`kabr-tools`**](https://github.com/dirtmaxim/kabr-tools) to create a layer between animal detection and manual correction of detected coordinates. These tools enable the identification of any inaccuracies in the automated process and provide a way to correct them in a timely and efficient manner. We also developed an interpolation tool that fills in any missed detections within a track, thereby increasing the overall tracking quality. This tool uses an algorithm that estimates the animal's location based on its previous movements, helping to fill in gaps where the automated detection may have failed. The complete data processing pipeline for the KABR dataset annotation is shown below:

![](assets/images/kabr-tools.png)

Our team heavily utilized [CVAT](https://github.com/opencv/cvat) to manually adjust bounding boxes detected by [YOLOv8](https://github.com/ultralytics/ultralytics), merge tracks produced by improved [SORT](https://arxiv.org/abs/1602.00763), and manually annotate behavior for extracted mini-scenes.

---

### **Examples**

The dataset includes a total of eight categories that describe various animal behaviors. These categories are `Walk`, `Graze`, `Browse`, `Head Up`, `Auto-Groom`, `Trot`, `Run`, and `Occluded`.

<div class="gifs"></div>

| **Walk** | ![](assets/gifs/examples/G0073-Walk.gif) | ![](assets/gifs/examples/ZP0632-Walk.gif) | ![](assets/gifs/examples/ZG0658-Walk.gif) |
| :---: | :---: | :---: | :---: |
| **Graze** | ![](assets/gifs/examples/ZG0658-Graze.gif) | ![](assets/gifs/examples/ZG0728-Graze.gif) | ![](assets/gifs/examples/ZP0554-Graze.gif) |
| **Browse** | ![](assets/gifs/examples/G0078-Browse.gif) | ![](assets/gifs/examples/G0097-Browse.gif) | ![](assets/gifs/examples/G0105-Browse.gif) |
| **Head Up** | ![](assets/gifs/examples/G0069-Head_Up.gif) | ![](assets/gifs/examples/ZG0668-Head_Up.gif) | ![](assets/gifs/examples/ZP0630-Head_Up.gif) |
| <span id="auto-groom">**Auto-Groom**</span> | ![](assets/gifs/examples/ZG0015-Auto-Groom.gif) | ![](assets/gifs/examples/ZP0578-Auto-Groom.gif) | ![](assets/gifs/examples/ZG0046-Auto-Groom.gif) |
| **Trot** | ![](assets/gifs/examples/ZG0217-Trot.gif) | ![](assets/gifs/examples/ZP0187-Trot.gif) | ![](assets/gifs/examples/ZP0198-Trot.gif) |
| **Run** | ![](assets/gifs/examples/G0068-Run.gif) | ![](assets/gifs/examples/ZP0192-Run.gif) | ![](assets/gifs/examples/ZP0600-Run.gif) |
| **Occluded** | ![](assets/gifs/examples/ZG0665-Occluded.gif) | ![](assets/gifs/examples/ZP0225-Occluded.gif) | ![](assets/gifs/examples/ZP0463-Occluded.gif) |

---

### **Experiments**

| Method | All | Giraffes | Plains Zebras | Grevy’s Zebras |
| :---: | :---: | :---: | :---: | :---: |
| I3D (16x5) | 53.41 | 61.82 | 58.75 | 46.73 |
| SlowFast (16x5, 4x5) | 52.92 | 61.15 | 60.60 | 47.42 |
| X3D (16x5) | **61.9** | **65.1** | **63.11** | **51.16** |

---

### **Demo**

| Method | Video |
| :---: | :---: |
| **X3D (16x5)** | ![](assets/videos/ZG0717.gif) |

---

### **Visualization**

By analyzing the gradient information flowing into the final convolutional layers of the network, [Grad-CAM](https://arxiv.org/abs/1610.02391) generates a heat map that highlights the regions of the image that contribute most significantly to the network's decision. In our case, Grad-CAM demonstrates that the neural network indeed prioritizes the exact animal in the center of the frame. There are two exceptions to consider: `Run` and `Occluded` categories. In the case of `Run`, the background changes very rapidly, which allows the network to identify it as `Run`. In the case of the `Occluded` category, where the animal is not visible within the frame, the network shifts its attention to focus on other objects present.

<div class="gifs"></div>

| ![](assets/gifs/grad-cams/walk_1.gif) | ![](assets/gifs/grad-cams/walk_2.gif) | ![](assets/gifs/grad-cams/walk_3.gif) | ![](assets/gifs/grad-cams/walk_4.gif) |
 :---: | :---: | :---: | :---: |
| ![](assets/gifs/grad-cams/graze_1.gif) | ![](assets/gifs/grad-cams/graze_2.gif) | ![](assets/gifs/grad-cams/graze_3.gif) | ![](assets/gifs/grad-cams/graze_4.gif) |
| ![](assets/gifs/grad-cams/browse_1.gif) | ![](assets/gifs/grad-cams/browse_2.gif) | ![](assets/gifs/grad-cams/browse_3.gif) | ![](assets/gifs/grad-cams/browse_4.gif) |
| ![](assets/gifs/grad-cams/head_up_1.gif) | ![](assets/gifs/grad-cams/head_up_2.gif) | ![](assets/gifs/grad-cams/head_up_3.gif) | ![](assets/gifs/grad-cams/head_up_4.gif) |
| ![](assets/gifs/grad-cams/trot_1.gif) | ![](assets/gifs/grad-cams/trot_2.gif) | ![](assets/gifs/grad-cams/trot_3.gif) | ![](assets/gifs/grad-cams/trot_4.gif) |
| ![](assets/gifs/grad-cams/run_1.gif) | ![](assets/gifs/grad-cams/run_2.gif) | ![](assets/gifs/grad-cams/run_3.gif) | ![](assets/gifs/grad-cams/run_4.gif) |
| ![](assets/gifs/grad-cams/occluded_1.gif) | ![](assets/gifs/grad-cams/occluded_2.gif) | ![](assets/gifs/grad-cams/occluded_3.gif) | ![](assets/gifs/grad-cams/occluded_4.gif) |

---

### **Format**

The KABR dataset follows the [Charades](https://arxiv.org/abs/1604.01753) format:

```
KABR
    /images
        /video_1
            /image_1.jpg
            /image_2.jpg
            ...
            /image_n.jpg
        /video_2
            /image_1.jpg
            /image_2.jpg
            ...
            /image_n.jpg
        ...
        /video_n
            /image_1.jpg
            /image_2.jpg
            /image_3.jpg
            ...
            /image_n.jpg
    /annotation
        /classes.json
        /train.csv
        /val.csv
```

The dataset can be directly loaded and processed by the [SlowFast](https://github.com/facebookresearch/SlowFast) framework.

---

### **Naming**

<pre>
<b>G</b>0XXX.X - Giraffes
<b>ZP</b>0XXX.X - Plains Zebras
<b>ZG</b>0XXX.X - Grevy's Zebras
</pre>

---

### **Information**

```
KABR/configs: examples of SlowFast framework configs.
KABR/annotation/distribution.xlsx: distribution of classes for all videos.
```

---

### **Scripts**

We provide `image2video.py` and `image2visual.py` scripts to facilitate exploratory data analysis.

```
image2video.py: Encode image sequences into the original video.
```

For example, `[image/G0067.1, image/G0067.2, ..., image/G0067.24]` will be encoded into `video/G0067.mp4`.

```
image2visual.py: Encode image sequences into the original video
with corresponding annotations.
```

For example, `[image/G0067.1, image/G0067.2, ..., image/G0067.24]` will be encoded into `visual/G0067.mp4`.

---

### **Acknowledgments and Disclosure of Funding**

This material is based upon work supported by the National Science Foundation under Award No. 2118240. The data was gathered at the Mpala Research Centre in Kenya, in accordance with Research License No. NACOSTI/P/22/18214. The data collection protocol adhered strictly to the guidelines set forth by the Institutional Animal Care and Use Committee under permission No. IACUC 1835F.

---

### **Citation**
```BibTeX
Coming soon...
```

<style>
p {
    text-align: justify !important;
}

tr, td, th {
    border: none !important;
}

div.gifs + table tr,
div.gifs + table td,
div.gifs + table th {
    border: none !important;
    padding: 1px !important;
  	line-height: 0px !important;
}

#youtube-table {
    display: block;
    width: 835px !important;
    margin-right: auto !important;
    margin-left: auto !important;
}

#youtube-table tr,
#youtube-table td,
#youtube-table th {
    padding: 15px !important;
}

#youtube-standard-label {
    display: inline;
}

#youtube-minimized-label {
    display: none;
}

#auto-groom {
    line-height: 25px !important;
}

td {
    padding: 0px !important;
}

tr:nth-child(even), th {
    background: #F8F8F8 !important;
}

#td-g {
    line-height: 0px !important;
    background: #5288AD !important;
}

#td-z {
    line-height: 0px !important;
    background: #AD7752 !important;
}

h1 {
    margin: 30px 0;
    font-size: 4em;
    letter-spacing: -1px;
}

hr {
    border-top: 1px solid #E3CD81FF !important;
}

@media screen and (max-width: 1024px) {
    #youtube-standard-label {
        display: none;
    }

    #youtube-minimized-label {
        display: inline;
    }

    #youtube-table {
        display: none;
    }
}
</style>