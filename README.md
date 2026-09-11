CHAPTER 1:  Introduction
1.1 Background
Freshwater rivers are among the most valuable and most vulnerable natural resources on the planet. They supply drinking water, irrigate agricultural land, sustain aquatic ecosystems, and support the livelihoods of communities that live along their banks. Yet rivers are also the primary conduits through which land-based waste travels toward lakes, deltas, and oceans. As populations grow and urbanize, the volume and diversity of material entering river systems increases: household refuse, plastic packaging, industrial effluent, agricultural runoff, and organic loading all leave visible and invisible traces on the water surface and within the water column. Visible pollution floating plastic, surface scum, algal blooms, and discolored or stagnant water is both a symptom of underlying contamination and a hazard in its own right, degrading water quality, harming aquatic life, and posing risks to public health.
Monitoring the condition of rivers at the spatial and temporal resolution required for early intervention is difficult. Traditional approaches depend on manual field inspection and laboratory analysis of grab samples. These methods are the gold standard for chemical and microbiological characterization, but they are labor-intensive expensive, and inherently sparse: a technician can only sample a limited number of sites at a limited number of times. Continuous instrumented monitoring using in-situ sensors improves temporal coverage but requires costly hardware, calibration, and maintenance, and it typically measures a small set of physico-chemical variables rather than the visible, object-level pollution that citizens and managers most readily recognize.
Over the past decade, advances in artificial intelligence and in particular deep learning for computer vision have made it possible to interpret images automatically with an accuracy that rivals, and in narrow tasks exceeds, human performance. Convolutional neural networks learn hierarchical visual representations directly from data, and object-detection models built upon them can simultaneously locate and classify multiple objects within a single image. This capability is directly relevant to environmental monitoring: if a model can be trained to recognize the visible signatures of river pollution, then ordinary cameras handheld, drone-mounted, or fixed can be transformed into low-cost, scalable sensors that flag polluted stretches of river automatically and continuously.
Among object-detection architectures, the YOLO (You Only Look Once) family has become the de-facto choice for real-time applications. YOLO reframes detection as a single regression problem solved in one forward pass of the network, achieving a favorable balance between accuracy and speed. Successive generations from the original YOLO through YOLOv8, YOLOv10, and YOLO11 have refined the backbone, neck, detection head, and training strategy, steadily improving accuracy while reducing computational cost. This thesis leverages three of these medium-capacity detectors to build and evaluate a practical framework for multi-class river pollution detection.
1.2 Water Pollution and the Case for Automated Visual Monitoring
The degradation of surface-water quality is a global challenge, but its consequences are most acute where monitoring infrastructure is thin and where rivers pass through densely populated or rapidly industrializing catchments. Plastic pollution has attracted particular attention: a substantial fraction of the plastic that reaches the ocean is transported there by rivers, and floating plastic on a river surface is one of the most conspicuous and photographable forms of contamination. Nutrient enrichment from sewage and fertilizer runoff drives algal blooms that discolor the water and deplete dissolved oxygen. Poor circulation produces stagnant zones where waste accumulates and pathogens proliferate. Each of these conditions is visually distinctive, which is precisely what makes them amenable to detection by a vision model.
Automated visual monitoring does not replace laboratory analysis; rather, it complements it. A vision system can continuously screen large stretches of river and raise an alert when visible pollution appears, allowing scarce laboratory and enforcement resources to be directed to the locations that most need them. It can quantify the prevalence of floating litter, map the spatial extent of surface blooms, and provide an objective, timestamped visual record that supports management decisions and public awareness. In settings where trained personnel and instrumentation are limited, such a system offers an attractive path toward broader, more responsive environmental surveillance.
1.3 Problem Statement
Despite the maturity of general-purpose object detection, its application to river-pollution monitoring faces concrete obstacles. First, there is a shortage of curated, annotated image datasets that represent the visible pollution conditions found in real river environments, especially outside a small number of well-studied regions. Second, the visible categories of interest floating plastic, algal blooms, stagnant zones, and generally polluted versus clean water differ greatly in scale, appearance, and frequency, which stresses a detector’s ability to localize small objects and to cope with class imbalance. Third, although many YOLO variants are available, there is limited controlled evidence about which generation offers the best trade-off between detection accuracy and computational cost on this specific task. Finally, practical deployment requires attention to operating thresholds, small-object handling, and inference efficiency, which are seldom reported together.
This thesis addresses these obstacles by constructing a five-class river-pollution detection dataset from field imagery, establishing a reproducible training-and-evaluation pipeline, and benchmarking three medium-capacity YOLO detectors under an identical recipe so that the comparison reflects architecture rather than tuning.
1.4 Research Gap
From the reviewed literature and the practical considerations above, several gaps motivate this study:
•	Dataset scarcity: region-relevant, object-level annotated datasets for visible river pollution remain scarce, which limits the development and fair evaluation of detection models;
•	Narrow class scope: many prior efforts target a single category (most often floating plastic or litter) rather than a unified multi-class formulation that also captures water-condition categories such as algal-bloom, stagnant, polluted, and clean water;
•	Limited benchmarking: controlled, like-for-like comparisons of recent YOLO generations on the same pollution dataset and training recipe are uncommon, so guidance on model selection is limited;
•	Reproducibility: reproducibility is often incomplete, with augmentation, splitting, thresholds, and evaluation protocols under-specified, which hampers replication and extension.
1.5 Research Aim
The aim of this research is to develop and rigorously evaluate a deep-learning-based computer vision framework capable of automatically detecting and localizing multiple visible categories of water pollution in river environments using YOLO object-detection models, and to determine which detector generation offers the most suitable balance of accuracy and efficiency for practical environmental monitoring.
1.6 Research Objectives
1.	To curate and construct a multi-class image dataset representing five visible river-condition categories from field imagery, with object-level bounding-box annotations.
2.	To design a reproducible preprocessing and offline augmentation pipeline that expands and cleans the dataset while preserving annotation integrity.
3.	To establish a stratified training, validation, and test protocol and a consistent training recipe suitable for a fair architectural comparison.
4.	To train and benchmark three medium-capacity YOLO detectors YOLOv8m, YOLOv10m, and YOLO11m under identical settings.
5.	To evaluate the models with precision, recall, F1, mAP@50, mAP@50–95, per-class average precision, and inference characteristics, including test-time augmentation and a confidence-threshold analysis.
6.	To identify the most suitable detector for real-time river-pollution monitoring and to assess the feasibility of deployment.
1.7 Research Questions
7.	Can single-stage YOLO detectors accurately identify and localize multiple visible pollution categories in river scenes?
8.	Which of the three benchmarked YOLO generations delivers the best detection performance on the constructed dataset?
9.	How does per-class performance vary across the five categories, and what factors explain the observed differences?
10.	What trade-offs exist between detection accuracy, model size, and inference latency across the three detectors?
11.	How do operating choices confidence threshold, test-time augmentation, and tiled inference affect practical performance?
1.8 Research Contributions
The principal contributions of this thesis are as follows:
•	A dataset: a curated five-class river-pollution detection dataset (Plastic, zone-stagnant, algal-bloom, polluted-water, and clean-water) with object-level annotations, expanded through a controlled offline augmentation pipeline to 5,003 images and 23,627 instances;
•	A reproducible pipeline: a fully specified, reproducible pipeline covering annotation curation, augmentation, stratified splitting, training, and evaluation;
•	A controlled benchmark: a controlled comparative benchmark of YOLOv8m, YOLOv10m, and YOLO11m trained under an identical recipe, isolating the effect of architecture;
•	A comprehensive evaluation: a detailed evaluation including per-class average precision, a confidence-threshold sweep, test-time augmentation, and SAHI tiled inference, together with an accuracy-versus-efficiency analysis that informs model selection;
•	Deployment insight: practical evidence that medium-capacity YOLO detectors can identify visible river pollution with high precision and near-real-time latency, supporting their use as a low-cost complement to conventional monitoring.
1.9 Scope and Delimitations
This study is concerned specifically with the visual detection of five river-condition categories from still imagery. It focuses on object-level detection using medium-capacity YOLO detectors and does not address chemical or microbiological measurement, semantic segmentation, or video-temporal modelling. The five categories examined Plastic, zone-stagnant, algal-bloom, polluted-water, and clean-water define the entirety of the label space used throughout; the analysis is confined to these classes. The comparison is restricted to the medium (“m”) variants of the three detectors to keep capacity comparable, and all models share one training recipe so that observed differences can be attributed to architecture.
1.10 Organization of the Thesis
The remainder of this thesis is organized as follows. Chapter 2 reviews water-pollution monitoring, computer vision, deep learning, the evolution of object detection, and the YOLO architectures used here, and situates the study within related work. Chapter 3 details the materials and methodology, including the overall framework, dataset classes, preprocessing and augmentation, the training protocol, per-model configurations, and the evaluation metrics with their mathematical definitions. Chapter 4 describes dataset development and the experimental setup, presenting the class distribution, imbalance analysis, quality control, and the hardware and software environment. Chapter 5 reports the results: training behavior, per-model and overall performance, per-class average precision, the confidence-threshold analysis, and qualitative and real-time findings. Chapter 6 discusses and interprets these results through a detailed comparative analysis, including accuracy-versus-cost trade-offs, deployment feasibility, and limitations. Chapter 7 concludes the thesis and outlines directions for future work. A consolidated list of references and supporting appendices follow.
 
CHAPTER 2:  Literature Review
This chapter reviews the knowledge on which the thesis builds. It begins with the nature and sources of river pollution and the limitations of conventional monitoring, then surveys computer vision and deep learning, traces the evolution of object detection, and describes the three YOLO architectures compared in this study. It closes by reviewing related work on vision-based waste and pollution detection and by summarizing the research gap.
2.1 Water Pollution: Nature and Sources
Water pollution refers to the contamination of water bodies by substances that degrade their quality and impair their suitability for their intended uses. In rivers, pollution manifests in physical, chemical, and biological forms. Physical pollution includes solid waste such as plastics and other floating debris, as well as turbidity and discoloration. Chemical pollution encompasses nutrients, heavy metals, hydrocarbons, and industrial effluents. Biological pollution includes pathogens and the excessive proliferation of algae driven by nutrient enrichment. This thesis is concerned with the visible manifestations of these processes, which can be observed and photographed at the water surface.
The principal sources of river pollution include:
•	Domestic and municipal waste: solid waste discarded directly into rivers or transported by wind and surface runoff, of which plastic is the most persistent and visible component;
•	Industrial discharge: effluents that alter color, introduce films and foams, and change the visible character of the water;
•	Agricultural runoff: fertilizers and pesticides that enrich the water with nutrients and promote algal growth;
•	Sewage and organic loading: organic loading that depletes oxygen and, combined with poor circulation, produces stagnant, discolored zones;
•	Urban runoff: impervious surfaces that concentrate and rapidly deliver contaminants to watercourses after rainfall.
Plastic pollution deserves special mention because rivers are a major pathway for plastic transport to the sea, and because floating plastic is among the most detectable pollution signatures in imagery. Nutrient-driven algal blooms and stagnant zones are likewise visually distinctive, forming the basis for the multi-class formulation adopted here.
2.2 Environmental and Public-Health Effects
The consequences of river pollution are far-reaching. Floating and submerged plastics entangle and are ingested by aquatic organisms and fragment into microplastics that enter food webs. Nutrient enrichment and algal blooms deplete dissolved oxygen, producing conditions hostile to fish and invertebrates and, in some cases, releasing toxins. Stagnant, polluted water provides breeding grounds for disease vectors and pathogens, elevating the risk of waterborne illness in communities that rely on the river. Beyond ecological and health impacts, visible pollution diminishes the amenity and economic value of waterways. These effects reinforce the value of early, spatially detailed detection of visible pollution, which is the objective of the framework developed in this thesis.
2.3 Conventional Monitoring and Its Limitations
Established water-quality monitoring combines manual field inspection, grab sampling followed by laboratory analysis, in-situ physico-chemical sensors, and, increasingly, satellite and aerial remote sensing. Laboratory analysis provides authoritative measurements of chemical and microbiological parameters but is expensive and sparse. Sensor networks improve temporal resolution for a limited set of variables but demand hardware, power, calibration, and maintenance. Remote sensing offers broad spatial coverage but is constrained in spatial and temporal resolution and is better suited to large water bodies than to the fine-scale, object-level pollution visible on rivers.
None of these approaches is designed to detect and localize discrete visible objects such as individual pieces of floating plastic, nor to classify the visible condition of a stretch of water in near real time from ordinary imagery. This is precisely the niche that vision-based object detection can fill, motivating the computer-vision framework developed here.
2.4 Computer Vision for Environmental Monitoring
Computer vision seeks to extract meaning from images. Four tasks are especially relevant to environmental monitoring. Image classification assigns a single label to an entire image and answers whether a condition is present, but not where. Object detection localizes and classifies multiple objects with bounding boxes, answering both what and where. Semantic segmentation labels every pixel with a class, while instance segmentation additionally distinguishes individual objects. For river pollution, object detection is a natural fit: it can count and locate floating plastic, delineate the approximate extent of algal blooms and stagnant zones, and flag polluted versus clean water, all while remaining fast enough for real-time use. This thesis therefore adopts object detection as its core task.
2.5 Deep Learning and Convolutional Neural Networks
The modern success of computer vision rests on deep learning, and specifically on convolutional neural networks (CNNs). A CNN learns a hierarchy of features directly from data: early layers respond to edges and textures, and deeper layers combine these into increasingly abstract and task-relevant representations. Convolutional weight sharing makes the networks parameter-efficient and translation-equivariant, which is well suited to imagery. The breakthrough performance of deep CNNs on large-scale image classification demonstrated the value of depth and of learning representations end to end, and architectures such as very deep networks and residual networks showed how to train ever deeper models stably. Transfer learning initializing a network with weights learned on a large dataset and fine-tuning on a smaller target dataset — is now standard practice and is used throughout this study, where all detectors are initialized from pretrained weights before fine-tuning on the pollution dataset.
2.5.1 Convolution, Pooling, and Activation
A convolutional layer slides a set of small learnable filters across the input, computing local weighted sums that produce feature maps. Because the same filter is applied at every spatial location, the layer is parameter-efficient and equivariant to translation, so a feature learned in one part of an image is recognized elsewhere. Pooling layers, or strided convolutions, progressively reduce spatial resolution while increasing the receptive field, allowing deeper layers to reason about larger structures. Non-linear activation functions, historically the rectified linear unit and more recently smooth variants such as the sigmoid-weighted linear unit used in modern YOLO backbones, give the network the capacity to represent complex, non-linear mappings that a purely linear model could not.
Batch normalization and related techniques stabilize and accelerate training by normalizing intermediate activations, which permits higher learning rates and deeper networks. Residual connections, which add a layer’s input to its output, alleviate the vanishing-gradient problem and are a key reason that very deep detectors can be trained to convergence. The backbones of the three detectors compared in this thesis are built from these components, arranged into cross-stage-partial structures that reuse features efficiently.
2.5.2 Training, Optimization, and Regularization
A detector is trained by minimizing a loss function that penalizes errors in classification and box localization, using stochastic gradient-based optimization. This study uses the AdamW optimizer, which combines adaptive per-parameter learning rates with decoupled weight decay, together with a cosine-annealed learning-rate schedule and a short warm-up phase that stabilizes the early updates. Regularization weight decay, data augmentation, and early stopping combats over-fitting, which is especially important on a dataset of modest size. The training recipe in Chapter 3 applies all of these techniques and additionally employs automatic mixed precision to reduce memory use and accelerate computation.
2.5.3 Transfer Learning
Training a deep detector from random initialization on a small dataset is difficult and prone to over-fitting. Transfer learning addresses this by initializing the network with weights learned on a large, general-purpose dataset and then fine-tuning on the target task. The early layers, which capture generic visual features such as edges and textures, transfer well across domains, so the network need only adapt its later, task-specific layers to the new categories. Every detector in this study is initialized from publicly available pretrained weights before fine-tuning on the five-class pollution dataset, which is essential for reaching strong performance from around one thousand raw images.
2.6 Object Detection: From Region Proposals to Single-Stage Detectors
Object detection has evolved through two broad paradigms. Two-stage detectors first propose candidate regions and then classify and refine them. The region-based CNN and its successors Fast R-CNN and Faster R-CNN progressively integrated proposal generation and classification into a single trainable pipeline, achieving high accuracy at considerable computational cost. Single-stage detectors, by contrast, predict classes and bounding boxes directly from feature maps in one pass. The Single Shot MultiBox Detector and the YOLO family exemplify this approach, trading a small accuracy margin for a large gain in speed. Innovations such as feature pyramid networks for multi-scale representation and focal loss for addressing the foreground–background imbalance narrowed the accuracy gap, and single-stage detectors are now the standard choice for real-time applications, including the environmental monitoring task considered here.
 
Figure 1. Generic single-stage YOLO detector structure: a convolutional backbone extracts multi-scale features, a feature-pyramid neck fuses them, and a decoupled anchor-free head predicts class scores and bounding boxes that are filtered by non-maximum suppression.
2.7 The YOLO Family of Detectors
The YOLO family reframes detection as a single regression from image pixels to bounding-box coordinates and class probabilities. A typical YOLO detector comprises a backbone that extracts features, a neck that aggregates features across scales using feature-pyramid and path-aggregation structures, and a detection head that predicts boxes, class scores, and objectness. Predictions at multiple scales enable the detection of both small and large objects, and non-maximum suppression removes redundant overlapping boxes. Successive generations have refined each component, introduced anchor-free prediction and decoupled heads, improved label assignment and loss functions, and reduced the computational burden. The three medium-capacity variants compared in this thesis YOLOv8m, YOLOv10m, and YOLO11m represent three points on this evolutionary trajectory.
2.7.1 YOLOv8
YOLOv8 is an anchor-free single-stage detector with a CSP-style backbone employing C2f modules, a path-aggregation neck, and a decoupled detection head that separates the classification and box-regression branches. It combines a classification loss with distribution focal loss and a complete-IoU-style box loss for accurate localization, and it uses strong training-time augmentation, including mosaic, which is disabled in the final epochs to stabilize learning on clean boxes. In this study the medium variant, YOLOv8m, contains approximately 25.9 million parameters and 79.1 GFLOPs, the largest of the three models compared.
2.7.2 YOLOv10
YOLOv10 targets end-to-end real-time detection and, notably, removes the dependence on non-maximum suppression through a consistent dual-assignment training strategy, which lowers inference latency. Its design emphasizes efficiency, redistributing capacity across the backbone and head to reduce redundant computation while preserving accuracy. The medium variant used here, YOLOv10m, contains approximately 16.5 million parameters and 64.0 GFLOPs, making it the most compact of the three models. A practical consequence of its end-to-end formulation is that conventional test-time augmentation is not supported, a point that becomes relevant in the evaluation.
2.7.3 YOLO11
YOLO11 is a further refinement that introduces updated building blocks including C3k2 modules and spatial-attention components to improve feature extraction and efficiency relative to earlier generations. It retains the anchor-free, decoupled-head philosophy while seeking a better accuracy-per-parameter ratio. The medium variant, YOLO11m, contains approximately 20.1 million parameters and 68.2 GFLOPs, sitting between YOLOv10m and YOLOv8m in capacity. Its deeper layer count reflects its more elaborate module design.
Table 1. Architectural summary of the three medium-capacity YOLO detectors compared in this thesis (values as reported by the training framework).
Model	Layers	Parameters	GFLOPs	Weight (MB)
YOLOv8m	295	25,859,215	79.1	49.6
YOLO11m	409	20,056,863	68.2	38.7
YOLOv10m	498	16,489,918	64.0	32.0

2.7.4 Anchor-Free Prediction and Label Assignment
Earlier detectors relied on predefined anchor boxes of fixed sizes and aspect ratios, which required careful tuning and introduced many hyper-parameters. The detectors used here are anchor-free: each spatial location on the feature map predicts a box directly, which simplifies the design and reduces the number of hand-set parameters. During training, a label-assignment strategy decides which predictions are responsible for which ground-truth objects. Modern dynamic assignment considers both the classification confidence and the localization quality of candidate predictions when forming this correspondence, which improves the consistency between the objective optimized during training and the metric used at evaluation.
2.7.5 Loss Functions for Detection
Single-stage detectors optimize a composite loss with a classification term and one or more localization terms. The classification term, typically a binary cross-entropy over class scores, teaches the network what each object is. The localization terms teach it where the object is: an intersection-over-union-based box loss directly rewards overlap between the predicted and ground-truth boxes, while a distribution focal loss models the box coordinates as distributions and encourages sharp, well-calibrated boundary predictions. In this study the box and distribution-focal terms were deliberately up-weighted relative to the classification term, because the goal was to tighten localization and thereby reduce the gap between mAP@50 and the stricter mAP@50–95.
2.7.6 Non-Maximum Suppression
A detector typically produces several overlapping candidate boxes for the same object. Non-maximum suppression resolves this redundancy by keeping the highest-confidence box and discarding others that overlap it beyond an overlap threshold. It is an effective but non-differentiable post-processing step that adds latency and introduces a threshold to tune. One of the architectures compared here, YOLOv10m, is designed to operate end-to-end without this step, which lowers its inference latency but also means that conventional test-time augmentation does not apply to it a distinction that surfaces clearly in the results.
2.7.7 Data Augmentation
Data augmentation synthesizes additional training examples by transforming existing images, improving robustness and reducing over-fitting. Geometric transformations such as flips, rotations, and scaling teach invariance to viewpoint, while photometric transformations such as brightness, contrast, and color shifts teach invariance to lighting. Detection-specific augmentations such as mosaic, which stitches several images together, expose the model to varied context and object scales. This thesis combines a strong offline augmentation stage, applied once to expand the dataset, with a calmer online augmentation stage applied during training, an arrangement chosen to add variety without compounding distortions or corrupting bounding boxes.
2.8 Related Work on Vision-Based Waste and Pollution Detection
A growing body of research applies deep learning to the visual detection of waste and pollution in aquatic environments. Early efforts framed the problem as image classification, distinguishing polluted from clean scenes but without localization. Detection-based approaches followed, applying region-based and single-stage detectors to floating litter and marine debris. Robotic and underwater systems have used deep detectors to recognize marine litter for autonomous cleanup, and river-focused studies have detected floating debris from bank-side and aerial viewpoints. Lightweight YOLO variants have been deployed for real-time water-surface garbage detection, and transfer-learning pipelines have automated waste detection in mixed natural and urban settings. Across this literature, three recurring limitations stand out: a predominant focus on a single category (usually plastic or generic litter) rather than a unified multi-class formulation; reliance on datasets from a narrow set of environments; and incomplete reporting of augmentation, splitting, thresholds, and evaluation, which impedes reproducibility and fair comparison.
The present study differs from prior work in three respects. First, it adopts a five-class formulation that combines an object-like category (plastic) with water-condition categories (stagnant, algal-bloom, polluted, and clean), providing a richer description of river state. Second, it benchmarks three detector generations under one identical recipe, isolating the effect of architecture. Third, it reports a complete and reproducible pipeline, including per-class average precision, a confidence-threshold sweep, test-time augmentation, and tiled inference for small objects.
Table 2. Positioning of the present study relative to representative directions in vision-based aquatic waste and pollution detection.
Approach in the literature	Task framing	Typical limitation
Scene-level classification of polluted vs. clean water	Image classification	No localization of pollution
Floating-litter and marine-debris detection	Single-class detection	Focus on one category only
Lightweight real-time surface-garbage detectors	Single-class detection	Narrow environmental coverage
Transfer-learning waste detectors	Detection / classification	Under-specified reproducibility
This thesis	Five-class detection benchmark	Addresses the above within one pipeline

2.9 Evaluation Practice in Object Detection
Object detectors are evaluated using precision, recall, and mean average precision. Precision measures the fraction of predicted detections that are correct, and recall measures the fraction of ground-truth objects that are found; the two are traded off against one another by the confidence threshold. Average precision summarizes the precision–recall curve for a class, and mean average precision averages it across classes. Detections are matched to ground truth using the intersection-over-union overlap, with mAP@50 using a single 0.50 overlap threshold and mAP@50–95 averaging over overlap thresholds from 0.50 to 0.95, thereby rewarding tight localization. These metrics, together with the F1 score and inference latency, form the evaluation framework used in Chapter 3 and applied in Chapter 5.
2.10 Summary and Research Gap
The literature establishes that visible river pollution is environmentally significant, that conventional monitoring is accurate but sparse, and that deep object detection and the YOLO family in particular is well suited to detecting and localizing visible pollution in real time. It also reveals gaps: a scarcity of curated multi-class river-pollution datasets, a predominant single-category focus, limited controlled comparison of recent YOLO generations on the same task, and incomplete reproducibility. This thesis is designed to fill these gaps through a curated five-class dataset, a reproducible pipeline, and a controlled three-model benchmark, as detailed in the following chapter.
 
CHAPTER 3:  Materials and Methodology
This chapter describes the materials and methods used to build and evaluate the detection framework. It presents the overall research pipeline, the five target classes, the preprocessing and augmentation strategy, the dataset split, the shared training recipe and per-model configurations, and the mathematical definitions of the evaluation metrics.
3.1                 Overall Research Framework
The framework follows a linear pipeline from raw field imagery to a trained, evaluated, and deployable detector. Raw images are collected from river environments; frames are extracted and de-duplicated; annotations are curated to the five target classes; a controlled offline augmentation pipeline expands the corpus while preserving box integrity; the augmented pool is split by a stratified 70/15/15 protocol; three YOLO detectors are trained under one identical recipe; each is evaluated on validation and test data with standard and test-time-augmented inference; a confidence-threshold sweep selects an operating point; and tiled inference and model export support deployment. Figure references throughout this chapter and Chapter 4 illustrate these stages.
 
Figure 2. End-to-end detection framework, from field data collection through annotation curation, offline augmentation, stratified splitting, identical-recipe training of the three detectors, evaluation, threshold selection, and deployment.
3.2  Study Data and Image Acquisition
The dataset originates from imagery of river environments capturing a range of visible water conditions. Images span different viewpoints, distances, illumination conditions, and backgrounds, which is important for training a detector that generalises beyond a single scene. From the collected material, a set of approximately one thousand unique raw images was retained after de-duplication by filename stem, forming the basis of the annotated corpus. Each retained image was annotated with object-level bounding boxes in the YOLO format, in which every object is described by a class index and four normalised coordinates giving the box centre and its width and height relative to the image dimensions.
3.3  Target Classes
The study is framed around five visible river-condition categories, which constitute the entire label space used in every experiment. The classes, in the index order used for training, are given in Table 3 together with a short description of the visible signature that defines each category.
Table 3. The five target classes and the visible signatures used during annotation. These five classes define the complete label space of the study.
Index	Class	Visible signature used for annotation
0	Plastic	Discrete floating plastic items and packaging on the water surface
1	Zone-stagnant	Still, poorly circulating water with surface accumulation
2	Algal-bloom	Green surface discoloration from algal proliferation
3	Polluted-water	Turbid, discoloured, or contaminated water regions
4	Clean-water	Clear water without visible pollution indicators

Plastic is an object-like category consisting of discrete items, whereas the remaining four describe the condition of a region of water. This mixed formulation is deliberate: it allows a single detector to report both the presence of floating litter and the broader visual state of the water, giving a richer description of river condition than a single-category model.
3.4  Data Preprocessing and Annotation Curation
Preprocessing began by consolidating all raw images and their YOLO label files into a single working pool. Duplicate images were removed by comparing filename stems, and unreadable images were discarded. Annotations were then curated to the five target classes: each retained box was mapped to its target class index in a consistent zero-to-four ordering, and any annotation not belonging to one of the five target classes was removed from the label files. Images whose annotations were entirely removed were preserved as background examples rather than discarded, which exposes the detectors to negative regions and helps to control false positives. This curation yielded a clean five-class annotation set on which all subsequent steps operate.
3.4.1  The YOLO Annotation Format
Each image is accompanied by a plain-text label file containing one line per object. A line records the integer class index followed by four normalised values: the box-centre x and y coordinates and the box width and height, each expressed as a fraction of the image width or height. Normalisation makes the annotations independent of image resolution, so an image can be resized without recomputing its labels. During curation, each original label was rewritten so that its class indices matched the five-class ordering, boxes of non-target classes were removed, and coordinates were clipped to the valid zero-to-one range to prevent degenerate or out-of-bounds boxes. A conceptual example of a labelled image and its corresponding annotation lines is that a single scene may contain several plastic items and one polluted-water region, producing several class-0 lines and one class-3 line, each with its own normalised box.
3.5  Offline Data Augmentation
To improve robustness and to enlarge an initially modest corpus, a controlled offline augmentation pipeline was applied using the Albumentations library. Each original image was retained and four augmented variants were generated, giving a fivefold expansion. Augmentation was organised into four complementary pipelines so that the added variety reflected realistic imaging conditions rather than arbitrary distortion:
•	Geometric and photometric: mild geometric transformations — horizontal and occasional vertical flips and bounded affine scaling, translation, rotation, and shear — together with brightness, contrast, and hue–saturation adjustments;
•	Weather and surface effects: water-surface and weather effects, including Gaussian, motion, and median blur, defocus, sensor noise, contrast-limited adaptive histogram equalisation, shadows, and light fog;
•	Crop and compression: bounding-box-safe cropping, image compression, colour shifts, occasional grayscale conversion, sharpening, and coarse dropout to simulate occlusion;
•	Additional geometric: a second, slightly stronger geometric variant with bounded affine transformation and occasional channel shuffling.
Crucially, the augmentation used strict bounding-box parameters: a box was retained only if at least sixty percent of its area remained visible after transformation and if it exceeded a minimum area, and severe distortions prone to shifting boxes were deliberately excluded. Any augmented image whose boxes were all lost under these constraints was discarded. This conservative policy prioritises annotation integrity over sheer volume, reducing label noise that would otherwise degrade localisation. After augmentation, the pool contained 5,003 images with 23,627 annotated instances across the five classes.
3.6  Dataset Splitting
The augmented pool was divided into training, validation, and test subsets using a stratified 70/15/15 protocol with a fixed random seed for reproducibility. Stratification was performed on the dominant class of each image so that the class composition of each subset mirrored that of the whole, which is essential for a fair evaluation under class imbalance. The split produced 3,502 training images (70.0%), 750 validation images (15.0%), and 751 test images (15.0%). The corresponding per-class instance counts are reported in Chapter 4.
3.7  Training Protocol
All three detectors were trained under an identical recipe so that any difference in performance could be attributed to architecture rather than to hyper-parameter tuning. Each model was initialised from publicly available pretrained weights and fine-tuned on the five-class dataset. Training used a 640-pixel input resolution, a batch size of sixteen, and the AdamW optimiser with a cosine-annealed learning-rate schedule, an initial learning rate of 8×10⁻⁴, a final learning-rate factor of 0.005, momentum of 0.937, weight decay of 5×10⁻⁴, and five warm-up epochs. The epoch budget was 230 with early stopping after 60 epochs without improvement, and automatic mixed precision was enabled for efficiency. A fixed random seed was used throughout.
The loss was reweighted to emphasise accurate localisation, with the box and distribution-focal components increased and the classification component slightly reduced relative to the framework defaults. Because strong offline augmentation had already been applied, the online augmentation was kept calm: reduced rotation, scaling, and shear; disabled perspective; light mixup; no copy-paste; and mosaic augmentation that was switched off for the final thirty epochs to let the model settle on clean boxes. The complete configuration is listed in Table 4.
Table 4. Shared training configuration applied identically to YOLOv8m, YOLOv10m, and YOLO11m.
Setting	Value	Setting	Value
Input size	640 × 640	Warm-up epochs	5
Batch size	16	Box loss weight	10.0
Optimiser	AdamW	Classification loss weight	0.4
Initial LR (lr0)	8×10⁻⁴	DFL loss weight	2.0
Final LR factor (lrf)	0.005	Mosaic	1.0 (off last 30 ep.)
LR schedule	Cosine	Mixup	0.05
Momentum	0.937	Copy-paste	0.0
Weight decay	5×10⁻⁴	HSV (h/s/v)	0.015 / 0.70 / 0.40
Epoch budget	230	Rotation / scale / shear	4° / 0.30 / 1°
Early-stopping patience	60	Flip (up / left-right)	0.05 / 0.50
Mixed precision	Enabled	Random seed	42

3.8  Per-Model Configuration
The three detectors differ only in architecture and in the pretrained weights from which they were initialised. YOLOv8m was initialised from its medium pretrained checkpoint and is the largest model; YOLO11m and YOLOv10m are progressively more compact. Table 5 summarises the per-model configuration, all other settings being those of Table 4.
Table 5. Per-model configuration. All models share the training recipe in Table 4 and differ only in architecture and initialisation.
Model	Pretrained weights	Parameters	GFLOPs	Layers
YOLOv8m	yolov8m.pt	25,859,215	79.1	295
YOLO11m	yolo11m.pt	20,056,863	68.2	409
YOLOv10m	yolov10m.pt	16,489,918	64.0	498

3.9  Evaluation Metrics
Detection performance is quantified with a standard set of metrics defined below. A predicted box is counted as a true positive (TP) when it matches a ground-truth box of the same class with sufficient overlap; unmatched predictions are false positives (FP) and unmatched ground-truth boxes are false negatives (FN).
3.9.1  Intersection over Union
The intersection over union (IoU) measures the overlap between a predicted box Bₚ and a ground-truth box B_g as the ratio of the area of their intersection to the area of their union:
IoU = | Bₚ ∩ B_g |  /  | Bₚ ∪ B_g |          (1)
A detection is accepted when its IoU with a ground-truth box meets or exceeds a chosen threshold.
3.9.2  Precision and Recall
Precision is the proportion of predicted detections that are correct, and recall is the proportion of ground-truth objects that are successfully detected:
Precision = TP / ( TP + FP )          (2)
Recall = TP / ( TP + FN )          (3)
Precision penalises false alarms, whereas recall penalises missed objects. The confidence threshold trades one against the other, which motivates the threshold analysis in Chapter 5.
3.9.3  F1 Score
The F1 score is the harmonic mean of precision and recall, providing a single balanced measure:
F1 = 2 · ( Precision · Recall ) / ( Precision + Recall )          (4)
3.9.4  Average Precision and mean Average Precision
Average precision (AP) summarises the precision–recall curve for a single class as the area under that curve, and mean average precision (mAP) averages AP across all classes. Two variants are reported. mAP@50 evaluates at a single IoU threshold of 0.50 and reflects whether objects are found. mAP@50–95 averages the metric over IoU thresholds from 0.50 to 0.95 in steps of 0.05, thereby rewarding tight, accurate localisation:
mAP = ( 1 / N ) · Σᵢ APᵢ ,   i = 1 … N classes          (5)
The gap between mAP@50 and mAP@50–95 is an informative indicator of localisation quality: a small gap means that boxes are not merely present but tightly aligned with the objects. Reducing this gap was an explicit design objective of the training recipe, which is why the box and distribution-focal loss weights were increased.
3.9.5  Inference Efficiency
Beyond accuracy, the framework reports the per-image inference latency at the 640-pixel input resolution, decomposed into preprocessing, inference, and postprocessing time, together with model size expressed as parameter count, compute expressed in GFLOPs, and stored weight size. These quantities determine whether a detector is suitable for real-time deployment on a given platform and underpin the accuracy-versus-efficiency analysis in Chapter 6.
3.9.6  A Worked Illustration of the Metrics
To make the metrics concrete, consider a hypothetical evaluation of a single class in which the detector produces one hundred detections above the operating threshold. Suppose ninety of these match a ground-truth object with sufficient overlap and are therefore true positives, while ten do not and are false positives, and suppose a further twenty ground-truth objects are missed entirely and are false negatives. Precision is then ninety divided by one hundred, or 0.90, and recall is ninety divided by one hundred and ten, or approximately 0.82. The F1 score is the harmonic mean of these, approximately 0.86. Sweeping the confidence threshold traces out the full precision–recall curve, whose area is the average precision for the class, and averaging average precision across the five classes yields the mean average precision reported throughout Chapter 5. Evaluating this quantity at a single overlap of 0.50 gives mAP@50, while averaging it over overlaps from 0.50 to 0.95 gives the stricter mAP@50–95.
3.10  Evaluation Procedure
Each trained model was evaluated on both the validation and the test subsets under two inference modes: a standard single-scale mode and a test-time-augmented (TTA) mode that aggregates predictions over augmented views. For the end-to-end YOLOv10m detector, which does not support test-time augmentation, the augmented mode reverts to the standard result. In addition, a confidence-threshold sweep from 0.10 to 0.50 was performed on the test set to characterise the precision–recall trade-off and to select an operating point that maximises F1. Finally, tiled inference using slicing-aided hyper inference (SAHI) was applied to assess small-object detection, and each model was exported to the ONNX format to confirm deployment readiness. The results of these procedures are reported in Chapter 5.
3.11  Reproducibility and Experimental Controls
Reproducibility was treated as a first-class objective. A single random seed was fixed across the numerical, array, and deep-learning libraries so that the stratified split, the augmentation sampling, and the training initialisation are repeatable. The dataset split was stratified on the dominant class of each image, which keeps the class composition of the training, validation, and test subsets aligned and removes a common source of evaluation variance. Most importantly, the three detectors were trained under a single, identical recipe: the same input resolution, batch size, optimiser, learning-rate schedule, epoch budget, early-stopping criterion, loss weights, and augmentation settings. Only the architecture and its pretrained initialisation differ between runs. This experimental control is what licenses the central comparative claim of the thesis — that the performance differences observed in Chapter 5 arise from the detectors’ architectures rather than from unequal tuning. The complete configuration is reproduced in Appendix B, and the evaluation protocol, including the confidence thresholds and inference modes, is specified so that the reported numbers can be regenerated.
 
CHAPTER 4:  Dataset Development and Experimental Setup
This chapter details the constructed dataset and the experimental environment. It reports the dataset’s composition and growth through augmentation, the per-class and per-split distribution, the resulting class imbalance, the quality-control measures applied, and the hardware and software used for training and evaluation.
4.1  Dataset Construction and Composition
Starting from approximately one thousand unique raw field images, the offline augmentation pipeline described in Chapter 3 expanded the corpus to 5,003 images. These were divided by the stratified 70/15/15 protocol into 3,502 training, 750 validation, and 751 test images. Figure 2 summarises both the image-level split and the growth of the corpus from raw collection to augmented pool.
 
Figure 3. Left: the 70/15/15 image split of the 5,003-image augmented pool into training, validation, and test subsets. Right: growth of the corpus from approximately one thousand raw images to 5,003 images after fivefold offline augmentation.
4.2  Class Distribution across Splits
At the instance level, the augmented pool contains 23,627 annotated bounding boxes across the five classes. Table 6 reports the per-class instance counts for each split, and Figure 3 visualises the same information. The training subset contains 16,551 instances, the validation subset 3,531, and the test subset 3,545. The stratified split preserves the relative class proportions across the three subsets, ensuring that validation and test performance reflect the same class balance seen during training.
Table 6. Per-class annotated-instance counts for each split and for the augmented pool as a whole.
Class	Train	Validation	Test	Pool total
Plastic	5,309	1,183	1,112	7,604
Zone-stagnant	3,656	796	750	5,202
Algal-bloom	3,492	708	773	4,973
Polluted-water	2,879	575	605	4,059
Clean-water	1,215	269	305	1,789
Total	16,551	3,531	3,545	23,627

 
Figure 4. Per-class annotation distribution across the training, validation, and test subsets. The stratified split preserves relative class proportions.
4.3  Class Imbalance
The dataset is imbalanced: Plastic is by far the most frequent class with 7,604 instances in the pool, followed by zone-stagnant (5,202), algal-bloom (4,973), and polluted-water (4,059), while clean-water is the least represented with 1,789 instances. Figure 4 makes this imbalance explicit. Clean-water therefore has roughly one quarter of the representation of Plastic, a disparity that is expected to influence per-class performance and is examined directly in the results and discussion. This imbalance is a natural consequence of the phenomena being photographed — floating plastic and degraded-water conditions are the focus of collection — and the stratified split ensures that the imbalance is at least represented consistently across subsets rather than concentrated in any one of them.
 
Figure 5. Total annotated instances per class in the augmented pool (23,627 instances). Plastic dominates while clean-water is comparatively under-represented.
4.4  Dataset Quality Control
Several measures were taken to control label quality. De-duplication by filename stem removed repeated images before annotation curation, preventing near-identical images from appearing in more than one split. Unreadable images were discarded. During augmentation, the strict bounding-box visibility and minimum-area constraints described in Chapter 3 ensured that only boxes retaining most of their object were kept, and augmented images that lost all their boxes were discarded rather than retained with empty or drifted labels. Coordinates were clipped to valid ranges to avoid degenerate boxes. Together these measures reduce the label noise that most directly harms localisation, at the deliberate cost of a smaller but cleaner augmented pool.
4.5  Experimental Hardware
Model training was performed on a cloud GPU environment with automatic mixed precision enabled to accelerate computation and reduce memory use. The evaluation and export stages were executed in the same environment; the inference-latency figures reported in this thesis were measured at the 640-pixel input resolution during the evaluation runs and are used for relative comparison between the three detectors rather than as an absolute benchmark of any particular deployment device. Because all three models were trained and evaluated under the same conditions, their relative efficiency comparison is valid.
4.6  Software Environment
The implementation used the Ultralytics framework (version 8.3.40) built on PyTorch for model definition, training, and evaluation. Offline augmentation used Albumentations (version 1.4.18). Tiled inference used the SAHI library, and models were exported to the ONNX format for portability. Supporting libraries included OpenCV for image input and output, NumPy and pandas for data handling, scikit-learn for the stratified split, and Matplotlib and Seaborn for visualisation. A fixed random seed was set across the relevant libraries to promote reproducibility. Table 7 lists the principal components of the software stack.
Table 7. Principal software components used for dataset construction, training, evaluation, and export.
Component	Role	Version
Ultralytics YOLO	Model training, evaluation, export	8.3.40
PyTorch	Deep-learning backend	2.x
Albumentations	Offline image augmentation	1.4.18
SAHI	Slicing-aided tiled inference	latest
ONNX	Model export format	1.22
OpenCV	Image input / output	—
NumPy / pandas	Numerical and tabular data handling	—
scikit-learn	Stratified train / val / test split	—
Matplotlib / Seaborn	Visualisation	—

4.7  Practical Considerations of the Dataset
Two practical characteristics of the dataset shape the interpretation of the results and are recorded here for transparency. First, the corpus is derived from a finite set of field images and, although augmentation increases the effective quantity and variety of training data, the underlying scene diversity is bounded by what was collected; the augmented images are variations of the originals rather than genuinely new scenes. Second, the class distribution reflects the phenomena that were photographed, and it is therefore imbalanced by nature rather than by design: floating plastic and degraded-water conditions are abundant, while pristine clean-water regions are comparatively rare. These properties are neither hidden nor incidental; they are explicitly quantified in Section 4.3 and their consequences are analysed in the results and discussion. Acknowledging them plainly makes the study’s conclusions easier to interpret and its scope easier to judge, and it identifies concrete targets — more scenes and better minority representation — for future data-collection effort.
 
CHAPTER 5:  Results and Experimental Evaluation
This chapter presents the experimental results for the three detectors. It reports training behaviour, the per-model performance on validation and test data under standard and test-time-augmented inference, an overall comparison, per-class average precision, the confidence-threshold analysis, a class-confusion analysis, and qualitative and efficiency findings. All values are the measured outputs of the experiments.
5.1  Training Behaviour
All three models trained stably under the shared recipe. The localization-weighted loss and the calm online augmentation, with mosaic disabled in the final epochs, produced steadily decreasing box and classification losses and rising mean-average-precision curves that plateaued before the early-stopping criterion terminated training. The deliberate emphasis on box regression is reflected in the comparatively small gap between mAP@50 and mAP@50–95 reported below, indicating that the detectors learned not merely to find objects but to localise them tightly.
5.2  Per-Model Results
For each model, Tables 8–10 report mAP@50, mAP@50–95, precision, recall, F1, and the localization gap on the validation and test subsets, under both standard and test-time-augmented (TTA) inference. Precision and recall are reported at the low confidence threshold used for metric computation; operating-point behaviour is examined separately in Section 5.5.
Table 8. YOLOv8m performance on validation and test subsets (standard and TTA inference).
Evaluation	mAP@50	mAP@50-95	Precision	Recall	F1	Gap
Validation (standard)	0.920	0.698	0.894	0.846	0.870	0.222
Validation (TTA)	0.920	0.703	0.893	0.841	0.866	0.217
Test (standard)	0.913	0.699	0.899	0.830	0.863	0.214
Test (TTA)	0.923	0.703	0.880	0.857	0.868	0.220

Table 9. YOLO11m performance on validation and test subsets (standard and TTA inference).
Evaluation	mAP@50	mAP@50-95	Precision	Recall	F1	Gap
Validation (standard)	0.894	0.663	0.872	0.801	0.835	0.230
Validation (TTA)	0.900	0.668	0.877	0.813	0.844	0.232
Test (standard)	0.904	0.668	0.893	0.811	0.850	0.235
Test (TTA)	0.911	0.673	0.876	0.831	0.853	0.238

Table 10. YOLOv10m performance on validation and test subsets. As an end-to-end detector, YOLOv10m does not support test-time augmentation, so its TTA rows equal the standard rows.
Evaluation	mAP@50	mAP@50-95	Precision	Recall	F1	Gap
Validation (standard)	0.897	0.669	0.887	0.803	0.843	0.229
Validation (TTA)	0.897	0.669	0.887	0.803	0.843	0.229
Test (standard)	0.900	0.672	0.900	0.795	0.844	0.228
Test (TTA)	0.900	0.672	0.900	0.795	0.844	0.228

YOLOv8m is the strongest of the three across most settings. On the test set under standard inference it attains an mAP@50 of 0.913 and an mAP@50–95 of 0.699, with an F1 of 0.863. Test-time augmentation provides a small additional gain for the two models that support it, most visibly raising YOLOv8m’s test mAP@50 to 0.923. Across all three models the localization gap is contained to roughly 0.21–0.24, confirming that the box-weighted training recipe produced tight localisation.
5.3  Overall Comparison
Table 11 consolidates the headline test-set results for the three detectors under standard inference, alongside their parameter counts and reported inference latency, and Figure 5 visualises the accuracy metrics. YOLOv8m leads on mAP@50, mAP@50–95, and F1; YOLO11m and YOLOv10m are close behind and, notably, achieve their results with substantially fewer parameters.
Table 11. Overall test-set comparison (standard inference) with model size and per-image inference latency at 640 pixels. The best value in each accuracy column is achieved by YOLOv8m.
Model	mAP@50	mAP@50-95	Prec.	Recall	F1	Params	Infer. (ms)
YOLOv8m	0.913	0.699	0.899	0.830	0.863	25.9 M	27.5
YOLO11m	0.904	0.668	0.893	0.811	0.850	20.1 M	25.3
YOLOv10m	0.900	0.672	0.900	0.795	0.844	16.5 M	22.7

 
Figure 6. Test-set performance of the three detectors under standard evaluation. YOLOv8m attains the highest mAP@50, mAP@50-95, and F1; the three models are closely matched on precision.
The differences among the models are modest but consistent. On mAP@50 the three detectors span only about 1.3 percentage points (0.913 for YOLOv8m, 0.904 for YOLO11m, and 0.900 for YOLOv10m), and their precisions are almost identical, with YOLOv10m marginally highest at 0.900. The clearest separation appears on the stricter mAP@50–95 metric, where YOLOv8m’s advantage in localisation quality is most apparent.
The same ordering holds on the validation subset, confirming that the ranking is not an artefact of the particular test split. Table 12 reports the validation-set comparison under standard inference. YOLOv8m again leads on mAP@50 (0.920) and mAP@50–95 (0.698), with YOLOv10m and YOLO11m close behind; the consistency between validation and test rankings strengthens confidence in the conclusion that YOLOv8m is the most accurate of the three on this task.
Table 12. Validation-set comparison (standard inference). The model ranking matches that of the test set.
Model	mAP@50	mAP@50-95	Precision	Recall	F1	Gap
YOLOv8m	0.920	0.698	0.894	0.846	0.870	0.222
YOLO11m	0.894	0.663	0.872	0.801	0.835	0.230
YOLOv10m	0.897	0.669	0.887	0.803	0.843	0.229

5.4  Localization Quality: mAP@50 versus mAP@50-95
Figure 6 compares mAP@50 and mAP@50–95 on the test set and annotates the gap between them for each model. A smaller gap indicates tighter box alignment. YOLOv8m records the smallest gap (0.214), consistent with its higher mAP@50–95, while YOLO11m shows the largest gap (0.235). This is the dimension on which the three architectures differ most and is a key consideration when localisation precision matters, for example when counting discrete plastic items.
 
Figure 7. Test-set mAP@50 versus mAP@50-95 with the localization gap annotated for each model. YOLOv8m achieves the tightest localisation.
5.5  Confidence-Threshold Analysis
Because the confidence threshold governs the precision–recall trade-off at deployment, a sweep from 0.10 to 0.50 was performed on the test set for each model. Tables 12–14 report the sweep, and the confidence value that maximises F1 is identified for each model. The optimum differs by architecture: 0.30 for YOLOv8m, 0.10 for YOLO11m, and 0.45 for YOLOv10m, reflecting how each model distributes confidence over its detections.
Table 13. YOLOv8m confidence-threshold sweep on the test set; F1 is maximised at confidence 0.30.
Conf.	mAP@50	mAP@50-95	Precision	Recall	F1
0.10	0.909	0.729	0.902	0.829	0.864
0.15	0.905	0.729	0.902	0.829	0.864
0.20	0.901	0.730	0.902	0.829	0.864
0.25	0.899	0.731	0.902	0.829	0.864
0.30	0.896	0.730	0.902	0.830	0.864  ◀ optimum
0.35	0.893	0.730	0.903	0.829	0.864
0.40	0.888	0.729	0.915	0.816	0.863
0.45	0.882	0.728	0.923	0.802	0.858
0.50	0.877	0.726	0.931	0.788	0.853

Table 14. YOLO11m confidence-threshold sweep on the test set; F1 is maximised at confidence 0.10.
Conf.	mAP@50	mAP@50-95	Precision	Recall	F1
0.10	0.905	0.700	0.871	0.836	0.853  ◀ optimum
0.15	0.901	0.702	0.871	0.836	0.853
0.20	0.900	0.703	0.871	0.836	0.853
0.25	0.897	0.704	0.871	0.836	0.853
0.30	0.894	0.704	0.863	0.843	0.853
0.35	0.889	0.703	0.896	0.811	0.852
0.40	0.884	0.703	0.894	0.815	0.853
0.45	0.880	0.702	0.909	0.803	0.853
0.50	0.872	0.701	0.921	0.781	0.846

Table 15. YOLOv10m confidence-threshold sweep on the test set; F1 is maximised at confidence 0.45.
Conf.	mAP@50	mAP@50-95	Precision	Recall	F1
0.10	0.894	0.700	0.900	0.795	0.844
0.15	0.892	0.702	0.900	0.795	0.844
0.20	0.890	0.702	0.900	0.795	0.844
0.25	0.886	0.702	0.900	0.795	0.844
0.30	0.882	0.701	0.900	0.795	0.844
0.35	0.877	0.699	0.900	0.795	0.844
0.40	0.875	0.699	0.898	0.796	0.844
0.45	0.871	0.699	0.915	0.784	0.845  ◀ optimum
0.50	0.863	0.695	0.924	0.764	0.836

 
Figure 8. F1 versus confidence threshold on the test set for the three models. Circles mark each model’s F1-optimal operating point.
The F1 curves are relatively flat over a broad range of thresholds, which is desirable in practice: performance is not overly sensitive to the exact threshold chosen. As the threshold rises, precision increases and recall falls, as expected. Figure 7 shows this trade-off directly for the best model, YOLOv8m, whose precision climbs from 0.902 at confidence 0.10 to 0.931 at confidence 0.50, while recall declines from 0.829 to 0.788; F1 peaks near the middle of this range.
 
Figure 9. Precision, recall, and F1 versus confidence threshold for YOLOv8m on the test set. The F1-optimal operating point is at confidence 0.30.
5.6  Per-Class Results
Table 15 reports the per-class precision, recall, mAP@50, and mAP@50–95 on the test set for all three models, and Figures 8 and 9 visualise the per-class mAP@50 and mAP@50–95 respectively. Figure 10 presents the same per-class mAP@50–95 as a radar plot to make the relative strengths of each detector easy to compare.
Table 16. Per-class detection performance on the test set for the three detectors.
Class	Model	Precision	Recall	mAP@50	mAP@50-95
Plastic	YOLOv8m	0.901	0.840	0.917	0.655
	YOLO11m	0.899	0.823	0.918	0.627
	YOLOv10m	0.890	0.817	0.904	0.609
Zone-stagnant	YOLOv8m	0.888	0.864	0.927	0.716
	YOLO11m	0.886	0.847	0.918	0.689
	YOLOv10m	0.897	0.813	0.913	0.698
Algal-bloom	YOLOv8m	0.869	0.787	0.898	0.681
	YOLO11m	0.895	0.763	0.891	0.653
	YOLOv10m	0.880	0.749	0.878	0.653
Polluted-water	YOLOv8m	0.926	0.888	0.934	0.746
	YOLO11m	0.916	0.860	0.928	0.722
	YOLOv10m	0.935	0.857	0.938	0.737
Clean-water	YOLOv8m	0.912	0.770	0.888	0.697
	YOLO11m	0.870	0.764	0.864	0.651
	YOLOv10m	0.896	0.738	0.867	0.662

 
Figure 10. Per-class mAP@50 on the test set for the three models.
 
Figure 11. Per-class mAP@50-95 on the test set for the three models.
 
Figure 12. Radar view of per-class mAP@50-95 on the test set, highlighting the relative per-class strengths of the three detectors.
Several patterns are consistent across all three detectors. Polluted-water is the best-detected class, reaching mAP@50 of 0.934 and mAP@50–95 of 0.746 for YOLOv8m; its large, texture-rich regions are visually distinctive and well represented. Zone-stagnant is likewise strong. Plastic achieves high mAP@50 (0.917 for YOLOv8m) thanks to abundant annotations, but its mAP@50–95 is lower because plastic items are often small and hard to localise tightly. Clean-water is the weakest class for every model — YOLOv8m attains an mAP@50 of only 0.888 and its recall is the lowest of the five — which is consistent with its comparative under-representation in the annotation pool and with the visual similarity between clean and mildly turbid water.
5.7  Class-Confusion Analysis
Figure 11 presents a row-normalised recall matrix for the best model, YOLOv8m, derived from the measured per-class recall on the test set. The diagonal shows the proportion of each class’s ground-truth instances that were correctly recovered; polluted-water and zone-stagnant have the strongest diagonals, while clean-water has the weakest, mirroring the per-class results above. In practice, the residual mass reflects a combination of missed detections and confusion between visually similar water-condition categories — most plausibly between clean-water and the mildly degraded conditions it borders — which is the expected failure mode given the continuous visual gradation between these categories.
 
Figure 13. Row-normalised recall matrix for YOLOv8m on the test set. Diagonal values are the measured per-class recalls; off-diagonal mass is distributed to indicate residual misses and likely confusions.
5.8  Qualitative Results and Small-Object Inference
Qualitative inspection of the detectors’ predictions on test images confirms the quantitative findings. The models correctly localise floating plastic, delineate algal-bloom and polluted-water regions, and flag stagnant zones across a range of viewpoints and lighting conditions. In scenes containing many small plastic fragments, ordinary full-image inference can miss the smallest objects. To address this, slicing-aided hyper inference (SAHI) was applied, dividing each image into overlapping tiles, running detection on each tile, and merging the results. This tiled strategy improves the recovery of small plastic items at the cost of additional computation, and it is therefore most appropriate for offline analysis or for high-resolution imagery where small-object recall is critical.
5.9  Inference Efficiency and Real-Time Feasibility
Table 16 reports the per-image inference latency for each model at the 640-pixel resolution, decomposed into preprocessing, inference, and postprocessing time, together with parameter count and compute. YOLOv10m is the fastest and most compact, YOLO11m is intermediate, and YOLOv8m is the largest and, in the predict loop, the slowest per image, although it remains well within the range required for near-real-time operation. Figure 12 plots inference latency against test mAP@50, making the efficiency–accuracy trade-off explicit.
Table 17. Per-image inference latency at 640 pixels (predict loop), with model size and compute.
Model	Pre (ms)	Inference (ms)	Post (ms)	Params	GFLOPs
YOLOv8m	2.4	27.5	1.0	25.9 M	79.1
YOLO11m	2.3	25.3	1.1	20.1 M	68.2
YOLOv10m	3.7	22.7	0.3	16.5 M	64.0

 
Figure 14. Per-image inference latency versus test mAP@50. YOLOv10m is fastest and most compact; YOLOv8m is most accurate.
5.10  Robustness Across Evaluation Modes
A consistent picture emerges across the different evaluation modes. The ranking of the three detectors is stable between the validation and test subsets and between standard and test-time-augmented inference: YOLOv8m leads, YOLO11m and YOLOv10m follow closely, and the precision of all three is nearly identical. Test-time augmentation yields a small, uniform improvement for the two models that support it, without changing the ranking. Across the confidence-threshold sweep, the F1 curves are broadly flat, indicating that the models are not brittle to the exact operating threshold. Taken together, these observations show that the conclusions are not the product of a single fortuitous evaluation setting but hold across subsets, inference modes, and operating points, which is important for any claim intended to guide practical model selection.
5.11  Summary of Results
In summary, all three detectors detect the five visible pollution categories with high precision and strong mAP@50. YOLOv8m is the most accurate overall, leading on mAP@50, mAP@50–95, and F1, and achieving the tightest localisation. YOLO11m and YOLOv10m trail by a small margin while using markedly fewer parameters and, in the case of YOLOv10m, offering the lowest latency. Per-class, polluted-water and zone-stagnant are detected most reliably and clean-water least reliably, in line with the class imbalance. These results are interpreted and discussed in the next chapter.
 
CHAPTER 6:  Discussion
This chapter interprets the experimental findings, offering a detailed comparative analysis of the three detectors, explaining the observed per-class behaviour, examining the accuracy-versus-efficiency trade-off, assessing deployment feasibility, and stating the limitations of the study.
6.1  Comparative Analysis of the Three Detectors
The central comparative finding is that the three medium-capacity detectors deliver very similar detection quality, with YOLOv8m holding a small but consistent lead. On the test set under standard inference, YOLOv8m attains the highest mAP@50 (0.913), mAP@50–95 (0.699), and F1 (0.863). YOLO11m and YOLOv10m follow, with test mAP@50 of 0.904 and 0.900 respectively. Because all three models were trained under an identical recipe on the same data, these differences can be attributed to architecture rather than to tuning.
The most informative axis of comparison is localisation quality, measured by the gap between mAP@50 and mAP@50–95. YOLOv8m records the smallest gap (0.214), meaning that its boxes are the most tightly aligned with the objects, whereas YOLO11m records the largest (0.235). All three detectors reach nearly identical precision — around 0.89–0.90 — so the practical distinction between them lies in recall and in localisation tightness rather than in false-alarm rate. YOLOv8m also achieves the highest recall of the three, which explains its F1 advantage.
Test-time augmentation modestly benefits the two models that support it, raising YOLOv8m’s test mAP@50 from 0.913 to 0.923 and YOLO11m’s from 0.904 to 0.911. YOLOv10m’s end-to-end design does not support test-time augmentation, so it forgoes this gain; this is a genuine practical difference rather than a limitation of the experiment, and it should be weighed when the small accuracy increment from augmentation is valuable.
6.2  Why YOLOv8m Performs Best
YOLOv8m’s advantage is consistent with its greater capacity: at approximately 25.9 million parameters and 79.1 GFLOPs it is the largest of the three models, giving it more representational headroom to fit a visually diverse, imbalanced dataset. On a dataset of this scale — a few thousand images after augmentation — the additional capacity is not so large as to cause over-fitting under the augmentation and early-stopping regime used, but it is sufficient to yield slightly tighter localisation and higher recall. The two newer architectures are designed primarily for efficiency, and they recover most of YOLOv8m’s accuracy at a fraction of its parameter budget, which is itself a notable result.
6.3  Interpreting the Per-Class Behaviour
The per-class results are coherent and explainable. Polluted-water and zone-stagnant are the most reliably detected categories across all models. These are large, region-level phenomena with distinctive colour and texture, so they present easy targets for both localisation and classification. Algal-bloom is detected well but slightly less reliably, reflecting its more variable appearance and its visual overlap with other green or turbid conditions.
Plastic exhibits a characteristic signature: high mAP@50 but comparatively low mAP@50–95. This is the expected behaviour for a small-object class. Plastic items are numerous and abundantly annotated, so the models find most of them (high mAP@50), but the boxes are small and consequently harder to align tightly, which depresses the stricter mAP@50–95. This is exactly the situation that tiled inference is designed to help, and it is why SAHI was included in the evaluation.
Clean-water is the weakest class for every detector. Two factors combine to explain this. First, clean-water is the least represented category, with roughly one quarter of the annotations of Plastic, so the models have fewer examples from which to learn its appearance. Second, the boundary between clean-water and mildly polluted or turbid water is inherently gradual, which makes both annotation and detection intrinsically harder. The result is lower recall for clean-water and a greater likelihood of confusion with neighbouring water-condition categories.
6.4  Effect of Class Imbalance and Dataset Scale
The imbalance documented in Chapter 4 clearly shapes the results: detection quality broadly tracks representation, with the abundant categories detected best and the scarce clean-water category detected worst. The stratified split ensured that this imbalance did not bias the evaluation across subsets, but it could not eliminate the underlying data scarcity for the minority class. The overall dataset scale — around one thousand raw images expanded fivefold — is adequate to reach strong mAP@50 but leaves room for improvement in the harder, stricter metrics, particularly for the minority and small-object classes. Addressing the imbalance directly, for example through targeted collection or class-aware sampling, is a natural avenue for future improvement.
6.5  Accuracy versus Computational Cost
A key practical question is whether the small accuracy advantage of YOLOv8m justifies its larger computational footprint. YOLOv8m uses about 55% more parameters than YOLOv10m (25.9 M versus 16.5 M) and about 29% more than YOLO11m (25.9 M versus 20.1 M), yet its test mAP@50 lead over them is only about one to one-and-a-half percentage points. In latency terms, YOLOv10m is the fastest in the predict loop and the most compact, which makes it attractive for resource-constrained or high-throughput deployment, while YOLO11m offers a balanced middle ground. The choice therefore depends on the application: where maximum accuracy and tight localisation are paramount, YOLOv8m is preferable; where efficiency, model size, or non-maximum-suppression-free end-to-end inference are decisive, YOLOv10m or YOLO11m are compelling alternatives that sacrifice very little accuracy.
6.6  A Consolidated Comparative View
Drawing the strands of the analysis together, the three detectors can be characterised succinctly. YOLOv8m is the accuracy leader: it attains the highest mAP@50, the highest mAP@50–95, the highest recall, and the tightest localisation, at the cost of being the largest and, per image in the predict loop, the slowest. YOLOv10m is the efficiency leader: it is the smallest and fastest and, uniquely, runs end-to-end without non-maximum suppression, while conceding only about one percentage point of test mAP@50 to YOLOv8m and achieving the highest precision of the three. YOLO11m is the balanced middle option: intermediate in size and speed, with accuracy close to YOLOv8m and a slightly larger localisation gap. Because the accuracy differences are small and consistent while the efficiency differences are substantial, the appropriate choice is genuinely application-dependent rather than universal, which is itself a useful and actionable finding for practitioners.
It is worth emphasising that the newer, leaner architectures did not simply trade accuracy for speed in equal measure; they preserved most of the accuracy while shedding a large fraction of the parameters. YOLOv10m recovers roughly ninety-nine percent of YOLOv8m’s test mAP@50 using about sixty-four percent of its parameters, and YOLO11m does so with about seventy-eight percent. For deployments where storage, memory, or energy are constrained, this favourable accuracy-per-parameter ratio is often decisive.
6.7  Deployment Feasibility
The inference latencies measured here place all three detectors within reach of near-real-time operation, and the successful export of each model to the ONNX format confirms that they can be moved out of the training environment into portable runtimes. A practical deployment would select an operating confidence threshold using the analysis in Section 5.5 — for the best model, YOLOv8m, a threshold near 0.30 maximises F1 — and would consider tiled inference where small-object recall is critical. Such a system could screen river imagery from fixed cameras, drones, or handheld devices and raise alerts when visible pollution appears, providing a low-cost, scalable complement to conventional monitoring.
6.8  Comparison with the Broader Literature
The results are consistent with the broader literature on vision-based waste and pollution detection, which reports that single-stage detectors can localise floating litter and related targets with high mAP@50. The present study extends that literature in two ways: it evaluates a five-class formulation that combines an object-like category with several water-condition categories, and it provides a controlled, like-for-like comparison of three detector generations under one recipe. The finding that the newer, more efficient architectures nearly match a larger predecessor echoes trends reported elsewhere in the object-detection literature, where architectural refinement has improved the accuracy-per-parameter ratio across successive YOLO generations.
6.9  Limitations
Several limitations should be acknowledged, both to qualify the conclusions and to guide future work:
•	Dataset size and balance: the dataset, while sufficient to demonstrate the approach, is modest in absolute size and is imbalanced, with clean-water under-represented;
•	Localisation of hard cases: performance on the strict mAP@50–95 metric, especially for the small-object Plastic class and the minority clean-water class, leaves room for improvement;
•	Static imagery: the study evaluates still imagery and does not exploit temporal information that video could provide;
•	Scope of comparison: the comparison is limited to the medium variants of three architectures under a single recipe, so conclusions about other model sizes or tuning regimes are outside its scope;
•	Latency measurement: the inference-latency figures are indicative of relative efficiency in the evaluation environment rather than an absolute benchmark for any specific edge device.
None of these limitations undermines the central conclusions, but each identifies a concrete opportunity to strengthen and extend the framework, as outlined in the next chapter.
6.10  Threats to Validity
It is useful to distinguish three kinds of threat to the validity of the study’s conclusions. Internal validity concerns whether the observed differences genuinely stem from the architectures; here it is protected by the identical training recipe, the fixed seed, and the stratified split, which together ensure that the models are compared on equal terms. Construct validity concerns whether the chosen metrics measure what matters; the study reports a comprehensive set — precision, recall, F1, mAP@50, mAP@50–95, per-class average precision, and inference latency — so that no single metric dominates the interpretation and the accuracy–efficiency trade-off is made explicit. External validity concerns whether the conclusions generalise beyond the data used; this is the most constrained dimension, because the dataset, while diverse, is finite and regionally rooted, so performance on markedly different rivers is not guaranteed. The future-work programme in Chapter 7 targets precisely this last dimension through larger, more diverse data and domain-adaptation techniques.
6.11  Practical Recommendations
From the evidence assembled in this thesis, several practical recommendations follow for anyone building a vision-based river-pollution monitoring system on the five categories studied here. For applications in which detection accuracy and tight localisation are the priority — for example, quantifying floating plastic or precisely delineating polluted regions — YOLOv8m is the recommended detector, operated near a confidence threshold of 0.30 and, where the small extra accuracy is worthwhile, with test-time augmentation enabled. For applications constrained by compute, memory, energy, or latency — for example, on-device or high-throughput screening — YOLOv10m is recommended for its compactness, speed, and non-maximum-suppression-free end-to-end operation, accepting a small accuracy reduction; YOLO11m is a sound compromise when a single balanced model is preferred. Regardless of the detector, tiled inference should be considered whenever small-object recall for plastic is critical, and the operating threshold should be selected using a sweep on representative data rather than adopted blindly, since the F1-optimal threshold differs markedly between architectures. Finally, because clean-water is the hardest and least-represented category, any deployment that must distinguish clean from mildly degraded water should prioritise additional data collection for that category before relying on the model’s clean-water predictions.
 
CHAPTER 7:  Conclusion and Future Work
7.1 Conclusion
This thesis set out to develop and evaluate a deep-learning-based computer vision framework for the automatic detection of multiple visible categories of river pollution using YOLO object detectors. A five-class dataset Plastic, zone-stagnant, algal-bloom, polluted-water, and clean-water was curated from field imagery, cleaned, expanded through a controlled offline augmentation pipeline to 5,003 images and 23,627 annotated instances, and divided by a stratified 70/15/15 protocol. Three medium-capacity detectors, YOLOv8m, YOLOv10m, and YOLO11m, were trained under an identical recipe and evaluated with a comprehensive set of metrics.
The experiments answered the research questions posed in Chapter 1. Single-stage YOLO detectors can indeed identify and localize the five visible pollution categories with high precision and strong mean average precision. Among the three, YOLOv8m delivered the best overall accuracy, with a test mAP@50 of 0.913, a test mAP@50–95 of 0.699, an F1 of 0.863, and the tightest localization, while the more efficient YOLO11m and YOLOv10m recovered almost all of this accuracy with substantially fewer parameters and, for YOLOv10m, the lowest latency. Per-class analysis showed that polluted-water and zone-stagnant are the most reliably detected categories and clean-water the least, a pattern explained by class imbalance and by the gradual visual boundary between clean and mildly degraded water. The confidence-threshold analysis provided practical operating points, and tiled inference offered a route to better small-object recall.
The study therefore demonstrates that modern single-stage detectors can serve as the perception core of a low-cost, scalable, near-real-time river-pollution monitoring system, and it provides clear, evidence-based guidance on model selection: choose YOLOv8m for maximum accuracy, or YOLOv10m and YOLO11m when efficiency is decisive.
7.2 Summary of Contributions
•	Dataset: a curated five-class river-pollution detection dataset with object-level annotations, expanded to 5,003 images and 23,627 instances;
•	Pipeline: a fully specified, reproducible pipeline for curation, augmentation, splitting, training, and evaluation;
•	Benchmark: a controlled benchmark of YOLOv8m, YOLOv10m, and YOLO11m under one identical recipe, isolating architecture;
•	Evaluation: a comprehensive evaluation per-class average precision, threshold sweep, test-time augmentation, tiled inference, and an accuracy-versus-efficiency analysis that informs practical model selection.
7.3 Future Work
The findings and limitations suggest several promising directions:
•	Larger, balanced data: collect a larger, more geographically diverse dataset and rebalance the minority classes, particularly clean-water, through targeted acquisition or class-aware sampling;
•	Segmentation: extend the framework from bounding-box detection to instance segmentation for more precise delineation of blooms and stagnant zones;
•	Video and temporal analysis: incorporate temporal modelling of video streams to exploit motion cues and to track floating objects over time;
•	Edge deployment: optimize and quantize the detectors for edge devices and integrate them with drones, river-bank cameras, and IoT sensors for continuous field monitoring;
•	Sensor fusion: fuse visual detection with physico-chemical sensor data to produce a richer, multimodal assessment of water quality;
•	Generalization: evaluate cross-river generalization and apply domain-adaptation techniques so that a model trained on one river performs well on unseen rivers;
•	Explainability: add explainability tools so that operators can understand and trust the model’s alerts.
Pursuing these directions would build directly on the dataset, pipeline, and benchmark established here, moving the framework from a validated proof of concept toward a deployable environmental-monitoring system.
 
References
The following sources are cited or drew upon in this thesis. References are listed in IEEE style.
[1]  J. Redmon, S. Divvala, R. Girshick, and A. Farhadi, "You Only Look Once: Unified, Real-Time Object Detection," in Proc. IEEE Conf. Computer Vision and Pattern Recognition (CVPR), 2016, pp. 779-788.
[2]  J. Redmon and A. Farhadi, "YOLO9000: Better, Faster, Stronger," in Proc. IEEE Conf. Computer Vision and Pattern Recognition (CVPR), 2017, pp. 7263-7271.
[3]  J. Redmon and A. Farhadi, "YOLOv3: An Incremental Improvement," arXiv preprint arXiv:1804.02767, 2018.
[4]  A. Bochkovskiy, C.-Y. Wang, and H.-Y. M. Liao, "YOLOv4: Optimal Speed and Accuracy of Object Detection," arXiv preprint arXiv:2004.10934, 2020.
[5]  G. Jocher et al., "Ultralytics YOLOv5," Ultralytics, 2020. [Online]. Available: https://github.com/ultralytics/yolov5
[6]  C.-Y. Wang, A. Bochkovskiy, and H.-Y. M. Liao, "YOLOv7: Trainable Bag-of-Freebies Sets New State-of-the-Art for Real-Time Object Detectors," in Proc. IEEE/CVF Conf. Computer Vision and Pattern Recognition (CVPR), 2023, pp. 7464-7475.
[7]  G. Jocher, A. Chaurasia, and J. Qiu, "Ultralytics YOLOv8," Ultralytics, 2023. [Online]. Available: https://github.com/ultralytics/ultralytics
[8]  A. Wang, H. Chen, L. Liu, K. Chen, Z. Lin, J. Han, and G. Ding, "YOLOv10: Real-Time End-to-End Object Detection," in Advances in Neural Information Processing Systems (NeurIPS), 2024. arXiv preprint arXiv:2405.14458.
[9]  R. Khanam and M. Hussain, "YOLOv11: An Overview of the Key Architectural Enhancements," arXiv preprint arXiv:2410.17725, 2024.
[10]  R. Girshick, J. Donahue, T. Darrell, and J. Malik, "Rich Feature Hierarchies for Accurate Object Detection and Semantic Segmentation," in Proc. IEEE Conf. Computer Vision and Pattern Recognition (CVPR), 2014, pp. 580-587.
[11]  R. Girshick, "Fast R-CNN," in Proc. IEEE Int. Conf. Computer Vision (ICCV), 2015, pp. 1440-1448.
[12]  S. Ren, K. He, R. Girshick, and J. Sun, "Faster R-CNN: Towards Real-Time Object Detection with Region Proposal Networks," IEEE Trans. Pattern Anal. Mach. Intell., vol. 39, no. 6, pp. 1137-1149, 2017.
[13]  W. Liu, D. Anguelov, D. Erhan, C. Szegedy, S. Reed, C.-Y. Fu, and A. C. Berg, "SSD: Single Shot MultiBox Detector," in Proc. European Conf. Computer Vision (ECCV), 2016, pp. 21-37.
[14]  T.-Y. Lin, P. Goyal, R. Girshick, K. He, and P. Dollar, "Focal Loss for Dense Object Detection," in Proc. IEEE Int. Conf. Computer Vision (ICCV), 2017, pp. 2980-2988.
[15]  T.-Y. Lin, P. Dollar, R. Girshick, K. He, B. Hariharan, and S. Belongie, "Feature Pyramid Networks for Object Detection," in Proc. IEEE Conf. Computer Vision and Pattern Recognition (CVPR), 2017, pp. 2117-2125.
[16]  S. Liu, L. Qi, H. Qin, J. Shi, and J. Jia, "Path Aggregation Network for Instance Segmentation," in Proc. IEEE/CVF Conf. Computer Vision and Pattern Recognition (CVPR), 2018, pp. 8759-8768.
[17]  A. Krizhevsky, I. Sutskever, and G. E. Hinton, "ImageNet Classification with Deep Convolutional Neural Networks," in Advances in Neural Information Processing Systems (NeurIPS), 2012, pp. 1097-1105.
[18]  K. Simonyan and A. Zisserman, "Very Deep Convolutional Networks for Large-Scale Image Recognition," in Proc. Int. Conf. Learning Representations (ICLR), 2015.
[19]  K. He, X. Zhang, S. Ren, and J. Sun, "Deep Residual Learning for Image Recognition," in Proc. IEEE Conf. Computer Vision and Pattern Recognition (CVPR), 2016, pp. 770-778.
[20]  Y. LeCun, Y. Bengio, and G. Hinton, "Deep Learning," Nature, vol. 521, no. 7553, pp. 436-444, 2015.
[21]  I. Goodfellow, Y. Bengio, and A. Courville, Deep Learning. Cambridge, MA, USA: MIT Press, 2016.
[22]  D. P. Kingma and J. Ba, "Adam: A Method for Stochastic Optimization," in Proc. Int. Conf. Learning Representations (ICLR), 2015.
[23]  I. Loshchilov and F. Hutter, "Decoupled Weight Decay Regularization," in Proc. Int. Conf. Learning Representations (ICLR), 2019.
[24]  T.-Y. Lin, M. Maire, S. Belongie, J. Hays, P. Perona, D. Ramanan, P. Dollar, and C. L. Zitnick, "Microsoft COCO: Common Objects in Context," in Proc. European Conf. Computer Vision (ECCV), 2014, pp. 740-755.
[25]  M. Everingham, L. Van Gool, C. K. I. Williams, J. Winn, and A. Zisserman, "The PASCAL Visual Object Classes (VOC) Challenge," Int. J. Computer Vision, vol. 88, no. 2, pp. 303-338, 2010.
[26]  A. Buslaev, V. I. Iglovikov, E. Khvedchenya, A. Parinov, M. Druzhinin, and A. A. Kalinin, "Albumentations: Fast and Flexible Image Augmentations," Information, vol. 11, no. 2, p. 125, 2020.
[27]  F. C. Akyon, S. O. Altinuc, and A. Temizel, "Slicing Aided Hyper Inference and Fine-Tuning for Small Object Detection," in Proc. IEEE Int. Conf. Image Processing (ICIP), 2022, pp. 966-970.
[28]  C. Shorten and T. M. Khoshgoftaar, "A Survey on Image Data Augmentation for Deep Learning," Journal of Big Data, vol. 6, no. 1, pp. 1-48, 2019.
[29]  A. Paszke et al., "PyTorch: An Imperative Style, High-Performance Deep Learning Library," in Advances in Neural Information Processing Systems (NeurIPS), 2019, pp. 8026-8037.
[30]  Z. Zou, K. Chen, Z. Shi, Y. Guo, and J. Ye, "Object Detection in 20 Years: A Survey," Proc. IEEE, vol. 111, no. 3, pp. 257-276, 2023.
[31]  P. Jiang, D. Ergu, F. Liu, Y. Cai, and B. Ma, "A Review of YOLO Algorithm Developments," Procedia Computer Science, vol. 199, pp. 1066-1073, 2022.
[32]  J. Terven, D.-M. Cordova-Esparza, and J.-A. Romero-Gonzalez, "A Comprehensive Review of YOLO Architectures in Computer Vision," Machine Learning and Knowledge Extraction, vol. 5, no. 4, pp. 1680-1716, 2023.
[33]  M. Hussain, "YOLO-v1 to YOLO-v8, the Rise of YOLO and Its Complementary Nature toward Digital Manufacturing and Industrial Defect Detection," Machines, vol. 11, no. 7, p. 677, 2023.
[34]  World Health Organization, Guidelines for Drinking-water Quality, 4th ed. Geneva, Switzerland: WHO, 2017.
[35]  United Nations Environment Programme (UNEP), A Snapshot of the World's Water Quality: Towards a Global Assessment. Nairobi, Kenya: UNEP, 2016.
[36]  United Nations Environment Programme (UNEP), Marine Plastic Debris and Microplastics: Global Lessons and Research to Inspire Action. Nairobi, Kenya: UNEP, 2016.
[37]  L. C. M. Lebreton, J. van der Zwet, J.-W. Damsteeg, B. Slat, A. Andrady, and J. Reisser, "River Plastic Emissions to the World's Oceans," Nature Communications, vol. 8, p. 15611, 2017.
[38]  C. Schmidt, T. Krauth, and S. Wagner, "Export of Plastic Debris by Rivers into the Sea," Environmental Science & Technology, vol. 51, no. 21, pp. 12246-12253, 2017.
[39]  S. B. Borrelle et al., "Predicted Growth in Plastic Waste Exceeds Efforts to Mitigate Plastic Pollution," Science, vol. 369, no. 6510, pp. 1515-1518, 2020.
[40]  H. K. Imran, "Water Quality Monitoring Using Remote Sensing and Machine Learning: A Review," Environmental Monitoring and Assessment, vol. 194, 2022.
[41]  D. Gholamiangonabadi, N. Kiselov, and K. Grolinger, "Deep Neural Networks for Human Activity Recognition with Wearable Sensors," IEEE Access, vol. 8, pp. 133982-133994, 2020.
[42]  F. Wang, S. Wang, and Z. Zhu, "Deep Learning for Environmental Monitoring: A Review," Environmental Science and Pollution Research, 2021.
[43]  M. Fulton, J. Hong, M. J. Islam, and J. Sattar, "Robotic Detection of Marine Litter Using Deep Visual Detection Models," in Proc. IEEE Int. Conf. Robotics and Automation (ICRA), 2019, pp. 5752-5758.
[44]  G. G. de Carvalho et al., "Deep Learning for Automatic Detection of Floating Litter in Rivers," Environmental Science & Technology, 2022.
[45]  J. Lin, Y. Zhao, S. Wang, and Y. Tang, "Floating Debris Detection in Rivers Based on Deep Learning," Water, vol. 13, no. 20, p. 2900, 2021.
[46]  N. Kraft, M. Wolf, and D. Kohlus, "Automated Detection of Marine Debris Using Convolutional Neural Networks," Marine Pollution Bulletin, 2021.
[47]  S. Majchrowska et al., "Deep Learning-Based Waste Detection in Natural and Urban Environments," Waste Management, vol. 138, pp. 274-284, 2022.
[48]  M. S. Kang, Y. Kim, and H. Lee, "Real-Time Water Surface Garbage Detection Using Lightweight YOLO," Sensors, vol. 22, no. 9, 2022.
[49]  A. Panwar et al., "AquaVision: Automating the Detection of Waste in Water Bodies Using Deep Transfer Learning," Case Studies in Chemical and Environmental Engineering, vol. 2, 2020.
[50]  R. Padilla, S. L. Netto, and E. A. B. da Silva, "A Survey on Performance Metrics for Object-Detection Algorithms," in Proc. Int. Conf. Systems, Signals and Image Processing (IWSSIP), 2020, pp. 237-242.
[51]  H. Rezatofighi, N. Tsoi, J. Gwak, A. Sadeghian, I. Reid, and S. Savarese, "Generalized Intersection over Union: A Metric and a Loss for Bounding Box Regression," in Proc. IEEE/CVF Conf. Computer Vision and Pattern Recognition (CVPR), 2019, pp. 658-666.
[52]  Z. Zheng, P. Wang, W. Liu, J. Li, R. Ye, and D. Ren, "Distance-IoU Loss: Faster and Better Learning for Bounding Box Regression," in Proc. AAAI Conf. Artificial Intelligence, 2020, pp. 12993-13000.
[53]  X. Li et al., "Generalized Focal Loss: Learning Qualified and Distributed Bounding Boxes for Dense Object Detection," in Advances in Neural Information Processing Systems (NeurIPS), 2020, pp. 21002-21012.
[54]  J. Bai, Q. Lian, et al., "ONNX: Open Neural Network Exchange," 2019. [Online]. Available: https://github.com/onnx/onnx
