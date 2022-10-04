# facade-segmentation

An exploration of the following paper: "The devil is in the labels: Semantic segmentation from sentences" ([Paper](https://arxiv.org/abs/2202.02002) | [GitHub](https://github.com/irfanICMLL/SSIW/tree/master)) fine-tuned on the CMP Facade Database ([Info](https://cmp.felk.cvut.cz/~tylecr1/facade/)).

## Introduction

This semantic segmentation paper trains a Segformer model to map each pixel in an image to an image embedding in the CLIP feature space. The CLIP language encoder is then used to map labels onto the same space so that per-pixel predictions can be made.

The main development in the paper is the use of full sentences over single labels to generate the text embeddings. Full sentences are reasoned to carry more semantic meaning and are therefore able to generate richer embeddings, leading to more accurate classification.

## Method

Using the 12 target labels for the CMP Facade dataset, I generated 3 different sets of embeddings. One using just the raw labels, a second using the short prompt: “this is an image of {label}” and a third using a full definition in line with the paper. These definitions were taken from Wikipedia as in the paper, except where there was no appropriate page, in which case dictionary definitions were used. The full definitions are listed in the appendix.

Baseline results were first taken using the provided model and then the decoder part of the Segformer was trained for 5 epochs for each set of embeddings. Results were assessed by calculating the average per-pixel accuracy score for all 378 images.

## Results

![results](https://github.com/theoclark/facade-segmentation/blob/main/results.png)

![predictions](https://github.com/theoclark/facade-segmentation/blob/main/predictions.png)

Whilst version 1 and 2 have similar accuracy scores for one-shot predictions, qualitatively there seems to be quite a difference (figure 1). In a one-shot setting the shorter the description for the embedding, the better the result appears to be. Version 2 and 3 both struggle to predict balconies and window predictions are patchy.

After fine tuning the difference between the three models is much smaller, though version 3 struggles to predict the presence of doors. This indicates that, with these definitions at least, using longer descriptions for the embeddings does not confer any advantage.

After fine tuning the decoder for 5 epochs, the difference in per-pixel accuracy between all 3 models was negligible (all 52%). Further training would no doubt yield incremental improvements for all models.
