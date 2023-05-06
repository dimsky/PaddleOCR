# Version: 2.0.0
FROM registry.baidubce.com/paddlepaddle/paddle:2.4.2-gpu-cuda11.2-cudnn8.2-trt8.0


ENV RE_MODEL_URL https://paddleocr.bj.bcebos.com/ppstructure/models/vi_layoutxlm/re_vi_layoutxlm_xfund_pretrained.tar

# PaddleOCR base on Python3.7
RUN pip3.7 install --upgrade pip -i https://mirror.baidu.com/pypi/simple

RUN pip3.7 install paddlehub --upgrade -i https://mirror.baidu.com/pypi/simple

RUN git clone https://ghproxy.com/https://github.com/dimsky/PaddleOCR.git /PaddleOCR

WORKDIR /PaddleOCR

RUN pip3.7 install -r requirements.txt -i https://mirror.baidu.com/pypi/simple

RUN mkdir -p /PaddleOCR/train_data/

# 下载数据
ADD https://paddleocr.bj.bcebos.com/ppstructure/dataset/XFUND.tar /PaddleOCR/train_data/
RUN tar xf /PaddleOCR/train_data/XFUND.tar -C /PaddleOCR/train_data

RUN mkdir -p /PaddleOCR/inference/

# 下载 SER 推理模型
ADD https://paddleocr.bj.bcebos.com/ppstructure/models/vi_layoutxlm/ser_vi_layoutxlm_xfund_infer.tar /PaddleOCR/inference/
RUN tar xf /PaddleOCR/inference/ser_vi_layoutxlm_xfund_infer.tar -C /PaddleOCR/inference/

# 下载 RE 推理模型
ADD https://paddleocr.bj.bcebos.com/ppstructure/models/vi_layoutxlm/re_vi_layoutxlm_xfund_infer.tar /PaddleOCR/inference/
RUN tar xf /PaddleOCR/inference/re_vi_layoutxlm_xfund_infer.tar -C /PaddleOCR/inference/

EXPOSE 8866

# CMD ["/bin/bash","-c","hub install deploy/hubserving/ocr_system/ && hub serving start -m ocr_system"]
CMD ["/bin/bash","-c","hub install deploy/hubserving/kie_ser/ && hub install deploy/hubserving/kie_ser_re/ && export CUDA_VISIBLE_DEVICES=0 && hub serving start -c deploy/docker/hubserving/kie/config.json"]