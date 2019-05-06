

# *toast*

<div align="center">
<img src="icon.png" , width="350"/>
</div>


[*toast*](http://138.197.206.129:5010/galaxies) ([Reis19](in prep)) is a data driven exploration tool of the Sloan Digital Sky Survey ([SDSS](https://www.sdss.org/)) galaxy spectra dataset. Currently it contains spectra of ~200,000 high SNR galaxies and quasars.


All these objects are presented on a single, interactive, 2D map of the data. Several such maps are available, on each map galaxies that share specific spectral properties are grouped together. By [interactively selecting](#objectSelection)  objects from the maps, the user can obtain and inspect groups of similar galaxies, examine the diversity of spectral features, and in general explore the data without relying on models. [*toast*](http://138.197.206.129:5010/galaxies) also includes the results of several [anomaly detection](#anomalyDetection) algorithms, making at least some of the most unusual galaxies readily available for inspection by the community.

## Code

The source code for [*toast*](http://138.197.206.129:5010/galaxies) is publicly available, it is written in python using the [bokeh](https://bokeh.pydata.org/en/latest/) library.

The machine learning techniques used in [*toast*](http://138.197.206.129:5010/galaxies) include Uniform Manifold Approximation and Projection ([UMAP](https://github.com/lmcinnes/umap)) for dimensionality reduction, in addition to several anomaly detection methods: (i) Unsupervised Random Forest ([Shi06](https://horvath.genetics.ucla.edu/html/RFclustering/RFclustering/RandomForestHorvath.pdf), [Baron16](https://arxiv.org/abs/1611.07526), [Reis18](https://arxiv.org/abs/1711.00022)), (ii) Isolation Forest ([Liu08](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.IsolationForest.html#id1)), (iii) Fisher Vectors ([Rotman19](in prep)) (iv) ... (v) ... .  --- is also used for ordering the galaxies before inspection (see [get order](#orderSection)).  The code we used to produce all these results is available at ... .

## Overview

### <a id="objectSelection"></a> Basic use case - interactive selection of objects

[*toast*](http://138.197.206.129:5010/galaxies) allows the user to view and select galaxies from a 2D embedding of the dataset. One of the supplied embedding is the familiar  Baldwin, Phillips & Telervich diagram ([BPT](http://adsabs.harvard.edu/abs/1981PASP...93....5B)). Using this embedding the user can select and view galaxies which have similar emission line ratios. Similarly, using the embeddings created by [UMAP](https://github.com/lmcinnes/umap), the user can select galaxies which have similar properties. In this case the similarity is an abstract similarity between the spectra of the galaxies. See the figure below for an example:

![lasso select](lasso_png.png)

The selected galaxies are now listed in the right side of the screen and we can inspect their spectra. We can also sort the selected galaxies by the property according to which the galaxies are colored. This is shown in the figure below:

![view selected galaxies](view_selected.png)

### buttons
In addition to the lasso selection tool, a few other interactive tools are available. The most relevant in our case are the [zoom in](#zoomInNote)  button, refresh, and save figure. These are shown in the Figure below:
![buttons](buttons.png)


## Different embeddings

It is possible to switch to a different embedding and see were the selected galaxies fall there. In our example we have selected strong Hd galaxies. This galaxies are similar in the region of the spectrum located around the Hd line, and indeed they are located near each other on a UMAP embedding of this region of the spectrum. Switching to a different embedding, that looks at a different region of the spectrum, we can check if this group of strong Hd galaxies  also have similar properties in other parts of the spectrum. Selecting an embedding is done with the following button:

![available embeddings](embeds.png)

The different UMAP embeddings refer to different regions of the spectrum. Galaxies that are located  near each other on the ```UMAP 4700-5100 A``` embedding are expected to have similar spectral properties in this specific region.  For a discussion and use cases of the merits of inspecting the data in subspaces see [Reis19](in prep).

## Coloring the data

The galaxies can also colored by several different properties:

![available colors](colors.png)

Most of these properties are taken from the [SDSS value added catalogs](https://www.sdss.org/dr14/data_access/value-added-catalogs/), the rest are calculated by us.

If you have suggestions for additional embeddings of color schemes, [let us know!](mailto:itamarreis@mail.tau.ac.il)

## <a id="anomalyDetection"></a> Anomaly detection

[*toast*](http://138.197.206.129:5010/galaxies) includes results of several anomaly detection algorithms. In scientific applications, anomaly detection is ultimately aimed at finding objects  we did not know existed and thus enabling new discoveries. To select the weirdest galaxies according to a given anomaly detection method, choose the method and press the ```Show anomalies``` button.

![anomaly detection](show_anomalies.png)

The example in the figure above will select the top 100 anomalies according to the Isolation Forest algorithm, show them on the current embedding, and list them on the right hand side. The result is shown in the Figure below with the spectrum of one of the detected anomalies.

![isolation forest anomalies](isf_anomalies.png)

A comparison between the different methods and a discussion on the need of more than a single anomaly detection method can be found in [Reis19](in prep).

## Additional features

###  Ordering the selected objects


The ```Get order``` button can optionally be used to order the selected objects before visually inspecting them. The ordering is based on the current embedding. Instead of going randomly from one selected object to another one might want to inspect objects with similar properties in groups. The ```Get order``` button effectively tries to find the shortest path to travel between the selected objects, on the embedding.
This can be useful for inspecting anomalies, as inspecting a group of anomalies with similar features can help us understand what are their unusual properties.

![get order button](order.png)

## Notes

### <a id="zoomInNote"></a> Adaptive viewer

Interactive graphs can get very heavy with more than  a few thousands of objects. For this reason we implemented an adaptive viewer which shows a random subset of the objects with a fixed size. The user can see additional objects by zooming in on a specific region. An example is shown in the Figure below:

![umap zoom example](umap_zoom.png)
