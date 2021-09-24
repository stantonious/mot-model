# mot-model

The model data used in training is; 20 samples of normalized IMU readings @ 30 Hz. (~ 2/3 of a second training samples). Normalization is accomplished by:

(Sa-Samin)/(Samax-Samin) * 255 and (Sg-Sgmin)/(Sgmax-Sgmin) * 255

where:

* Sa - Accelerometer XYZ samples
* Sg - Gyroscope XYZ samples
* Samin/max - Accelerometer min/max values (found by inspection)
* Sgmin/max - Gyroscope min/max values (found by inspection)

The model data is labeled as follows:

* 0 - Rest
* 1 - Forward
* 2 - Backward
* 3 - Left turn
* 4 - Right turn
* 5 - Up (jump)
* 6 - Down (squat)
* 7 - Left side step
* 8 - Right side step


The model is a simple Convolutional Neural Network with the following architecture:

Model: "sequential"
```
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
conv2d (Conv2D)              (None, 20, 6, 16)         208       
_________________________________________________________________
max_pooling2d (MaxPooling2D) (None, 6, 2, 16)          0         
_________________________________________________________________
dropout (Dropout)            (None, 6, 2, 16)          0         
_________________________________________________________________
conv2d_1 (Conv2D)            (None, 6, 2, 16)          1040      
_________________________________________________________________
max_pooling2d_1 (MaxPooling2 (None, 2, 2, 16)          0         
_________________________________________________________________
dropout_1 (Dropout)          (None, 2, 2, 16)          0         
_________________________________________________________________
flatten (Flatten)            (None, 64)                0         
_________________________________________________________________
dense (Dense)                (None, 16)                1040      
_________________________________________________________________
dropout_2 (Dropout)          (None, 16)                0         
_________________________________________________________________
dense_1 (Dense)              (None, 9)                 153       
=================================================================
Total params: 2,441
Trainable params: 2,441
Non-trainable params: 0
Training Results
```
The following are the F1 scores per label. A .95 F1 composite accuracy was achieved.
```
class   precision    recall  f1-score   support             
0       1.00      0.95      0.98        44            
1       0.90      0.94      0.92        95            
2       0.93      0.91      0.92       109            
3       1.00      1.00      1.00        52            
4       1.00      1.00      1.00        41            
5       1.00      1.00      1.00        38            
6       0.99      1.00      0.99        71            
7       0.88      0.91      0.89        54            
8       0.95      0.91      0.93        45      

accuracy                               0.95       549    
macro avg          0.96      0.96      0.96       549 
weighted avg       0.95      0.95      0.95       549
Deploy Model to device
```
