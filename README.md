### Split&Merge: Table Recognition with Pytorch

An implementation of Table Recognition Model Split&Merge in Pytorch.  Split&Merge is an efficient convolutional neural network architecture for recognizing table structure from images. For more detail, please check the paper from ICDAR 2019: Deep Splitting and Merging for Table Structure Decomposition

## Usage

**Clone the repo:**

```
git clone https://github.com/solitaire2015/Split_Merge_table_recognition.git
pip install -r requirements.txt
```

**Prepare the training data:**

The input of the Split model and Merge model should be an image which has one channels or three channels,  I used three channels image, ones channel is gray scale image, the other two are segmentation mask in vertical and horizontal direction. You can use gray scale image only by setting number of channels to 1 and dataset suffix to `.jpg`.

<u>Split model</u>

The ground truth of Split model is loaded from a `.json`  file, the structure of the file is like:

``````
{
“img_1”:{"rows":[0,0,1,0,1,1],"columns":[1,0,0,1,1,1,1]},
“img_2”:{"rows":[0,0,1,0,1,1],"columns":[1,0,0,1,1,1,1]}
}
``````

Where `row`  indicates if it's a line in corresponding row of the image, the length of `row` is height of the image. `columns` indicates corresponding column the length is width of the image.

<u>Merge model</u>

The ground truth of Merge is loaded from a `.json` file, here is the structure:

``````
{
“img_1”:{ "rows": [0.1891891891891892, 0.3918918918918919, 0.5945945945945946, 0.7837837837837838], 
	  "columns": [0.07102803738317758, 0.2897196261682243, 0.35514018691588783, 0.4523364485981308, 0.5514018691588785,                                   0.6317757009345795, 0.7140186915887851, 0.8130841121495327, 0.9102803738317757],
	  "h_matrix":[[0,0,0],
		      [1,0,1]]},
	  "v_matrix":[[0,1,0],
		      [0,0,1]]}
}
``````

where the `h_matrix` indicates where the cells should be merged with it's right neighborhood, `v_matrix` indicates where the cells should be merged with it's bottom neighborhood, check more detail from the original paper, `h_matrix` and `v_matrix` are `R`  and `D` in that paper.

There are two scripts in `data_utils` folder to generate training data.

**Training**

<u>Train Split model :</u>

``````
python split/train.py --img_dir your image dir -- json_dir your json file -- saved_dir where you want to save model --val_img_dir your validation image dir -- val_json your validation json file
``````

<u>Train Merge model:</u>

``````
python merge/train.py --img_dir your image dir -- json_dir your json file -- saved_dir where you want to save model --val_img_dir your validation image dir -- val_json your validation json file
``````

<u>Run the predict script:</u>

``````
jupyter notebook
``````

Open `Split_predict.ipynb` and `Merge_predict.ipynb` in your browser.

## Result

I didn't test the model on public ICDAR table recognition competition dataset, I test the model on my private dataset and got 97% F-score, you can test on ICDAR dataset by yourself.

Here are some example:

## Images

![](images/split_input.png)

Fig1. Original image

![](images/split_output.png)

Fig2. Split result

![](images/merge_example.png)

Fig3. one Merge result.

