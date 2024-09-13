# Version: 2.0.0
FROM paddlepaddle/paddle:2.5.1

# PaddleOCR base on Python3.7
RUN pip3.7 install --no-cache-dir --upgrade pip -i  https://pypi.tuna.tsinghua.edu.cn/simple

RUN pip3.7 install --no-cache-dir paddlehub --upgrade -i  https://pypi.tuna.tsinghua.edu.cn/simple

RUN pip3 uninstall -y astroid

RUN pip3 install astroid==2.12.2

RUN git clone https://gitee.com/PaddlePaddle/PaddleOCR.git /PaddleOCR

WORKDIR /PaddleOCR

RUN sed -i 's/1.4.10/1.3.1/g' requirements.txt

RUN pip3.7 install --no-cache-dir  -r requirements.txt -i  https://pypi.tuna.tsinghua.edu.cn/simple

RUN pip uninstall -y protobuf &&  pip install protobuf==3.20.2 -i  https://pypi.tuna.tsinghua.edu.cn/simple

RUN mkdir -p /PaddleOCR/inference/

# Download orc detect model(light version). if you want to change normal version, you can change ch_ppocr_mobile_v2.0_det_infer to ch_ppocr_server_v2.0_det_infer, also remember change det_model_dir in deploy/hubserving/ocr_system/params.py）
ADD https://paddleocr.bj.bcebos.com/PP-OCRv3/chinese/ch_PP-OCRv3_det_infer.tar /PaddleOCR/inference/
ADD https://paddleocr.bj.bcebos.com/dygraph_v2.0/ch/ch_ppocr_mobile_v2.0_cls_infer.tar /PaddleOCR/inference/
ADD https://paddleocr.bj.bcebos.com/PP-OCRv3/chinese/ch_PP-OCRv3_rec_infer.tar /PaddleOCR/inference/


RUN tar xf /PaddleOCR/inference/ch_PP-OCRv3_det_infer.tar -C /PaddleOCR/inference/
RUN tar xf /PaddleOCR/inference/ch_ppocr_mobile_v2.0_cls_infer.tar -C /PaddleOCR/inference/
RUN tar xf /PaddleOCR/inference/ch_PP-OCRv3_rec_infer.tar -C /PaddleOCR/inference/


EXPOSE 8866

CMD ["/bin/bash","-c","hub install deploy/hubserving/ocr_system/ && hub serving start -m ocr_system"]



