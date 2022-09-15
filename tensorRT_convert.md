## pytorch to TensorRT conversion

### Export yolov5 for batch inference  (e.g. For input size 640x640 and batch size 4)  
1. using yolov5 export file, `python3 export.py --weights yolov5m_shear.pt --include onnx --data coco128.yaml --device 0 --batch-size 4`    
2. Convert batch parameter to 'b' using models/dynmic.py.    
3. use following trtexec command for conversion (change the input resolution and batch size accordigly)    
`trtexec --onnx=dynamic.onnx --fp16 
                            --minShapes=images:1x3x640x640 
                            --optShapes=images:4x3x640x640 
                            --maxShapes=images:8x3x640x640 
                            --useCudaGraph 
                            --saveEngine= < output name >_dynamic.engine`  

### Export yolov5 for inference  
1. using yolov5 export file,`python3 export.py --weights yolov5m_shear.pt --include onnx --data coco128.yaml --device 0`   
2. `trtexec --onnx=yolov5m.onnx --fp16 --saveEngine=yolov5m_static.engine --useCudaGraph`        


For batch inference,

1. Alaways use a dynamic engine file.    
2. In config.py  
    - Make the TensorRT.batch `True`  
    - Set the correct batch size of the converted engine file in TensorRT.batch_size
3. Make shure the camera fps is set to a divisible value of the batch_size. 
