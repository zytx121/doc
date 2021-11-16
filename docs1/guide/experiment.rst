Run the Experiment
===================


Train
-------------------
* If you want to train your own dataset, please note:
   1) Select the detector and dataset you want to use, and mark them as ``#DETECTOR`` and ``#DATASET`` (such as ``#DETECTOR=retinanet`` and ``#DATASET=DOTA``)
   2) Modify parameters (such as ``CLASS_NUM``, ``DATASET_NAME``, ``VERSION``, etc.) in ``$PATH_ROOT/libs/configs/#DATASET/#DETECTOR/cfgs_xxx.py``
   3) Copy ``$PATH_ROOT/libs/configs/#DATASET/#DETECTOR/cfgs_xxx.py`` to ``$PATH_ROOT/libs/configs/cfgs.py``
   4) Add category information in ``$PATH_ROOT/libs/label_name_dict/label_dict.py``
   5) Add data_name to ``$PATH_ROOT/dataloader/dataset/read_tfrecord.py``

* **Make tfrecord**
If image is very large (such as DOTA dataset), the image needs to be cropped. Take DOTA dataset as a example:
::

   cd $PATH_ROOT/dataloader/dataset/DOTA
   python data_crop.py


If image does not need to be cropped, just convert the annotation file into xml format, refer to `example.xml <https://github.com/yangxue0827/RotationDetection/blob/main/example.xml>`_.
::

   cd $PATH_ROOT/dataloader/dataset/
   python convert_data_to_tfrecord.py --root_dir='/PATH/TO/DOTA/'
                                      --xml_dir='labeltxt'
                                      --image_dir='images'
                                      --save_name='train'
                                      --img_format='.png'
                                      --dataset='DOTA'

* **Start training**
::

   cd $PATH_ROOT/tools/#DETECTOR
   python train.py

Train and Evaluation
----------------------
* For large-scale image, take DOTA dataset as a example (the output file or visualization is in ``$PATH_ROOT/tools/#DETECTOR/test_dota/VERSION``):
::

   cd $PATH_ROOT/tools/#DETECTOR
   python test_dota.py --test_dir='/PATH/TO/IMAGES/'
                       --gpus=0,1,2,3,4,5,6,7
                       -ms (multi-scale testing, optional)
                       -s (visualization, optional)
                       -cn (use cpu nms, slightly better <1% than gpu nms but slower, optional)

or (recommend in this repo, better than multi-scale testing)
::

   python test_dota_sota.py --test_dir='/PATH/TO/IMAGES/'
                            --gpus=0,1,2,3,4,5,6,7
                            -s (visualization, optional)
                            -cn (use cpu nms, slightly better <1% than gpu nms but slower, optional)

.. note::
   In order to set the breakpoint conveniently, the read and write mode of the file is' a+'. If the model of the same ``#VERSION`` needs to be tested again, the original test results need to be deleted.

* For small-scale image, take HRSC2016 dataset as a example:
::

   cd $PATH_ROOT/tools/#DETECTOR
   python test_hrsc2016.py --test_dir='/PATH/TO/IMAGES/'
                           --gpu=0
                           --image_ext='bmp'
                           --test_annotation_path='/PATH/TO/ANNOTATIONS'
                           -s (visualization, optional)

* Tensorboard
::

   cd $PATH_ROOT/output/summary
   tensorboard --logdir=.

.. image:: ../../images/images.png
.. image:: ../../images/scalars.png
