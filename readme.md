<h3> 0. 개요</h3>

실행 환경 : Google Colab, Jupyter Notebook 

프레임워크 : tensorflow2 

서울시립대학교 대학 혁신 지원 사업 - 학생 미래 설계 학기 자기주도 연구 프로젝트 

주제 : 딥러닝을 활용한 암호 알고리즘 분석



대칭키 암호의 경우, 마스터키 기준 암호화 키와 복호화 키가 동일하다. 또한 블록 암호의 경우, 블록 별로 암호화를 진행하기 때문에 이미지 암호화의 경우 육안으로 어느정도 어떤 모드를 사용했는지 구분이 가능하다. 이를 딥러닝을 활용하여 판별하는 프로젝트를 진행한다.

추가 설명 : https://ghqls0210.tistory.com/category/%EC%95%94%ED%98%B8%ED%95%99/%EC%9E%90%EA%B8%B0%EC%A3%BC%EB%8F%84%EC%97%B0%EA%B5%AC%20%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8



<h3> 1. 데이터셋 및 소스코드</h3>

| 디렉토리       | 이름                                        | 설명                                                         |
| -------------- | ------------------------------------------- | ------------------------------------------------------------ |
| data_making    | crawling.ipynb                              | 데이터셋을 위한 이미지 크롤링                                |
|                | img_data_making.ipynb                       | 크롤링한 자료 type, shape 일치시키기                         |
|                | img_encrypt.ipynb                           | 이미지 데이터 암호화 (AES128, ECB, CBC, OFB 모드) (1000장)   |
|                | CBC_img, ECB_img, OFB_img                   | AES128을 이용해 암호화된 이미지 파일 디렉토리 (256*256 pixel) |
|                | des_encrypt_img.ipynb                       | 이미지 데이터 암호화 (single DES, ECB, CBC, OFB 모드) (1000장) |
|                |                                             |                                                              |
| 128pixel_model | 128 modle test.ipynb                        | 128 픽셀 ECB, CBC, OFB 데이터 모델 및 학습 파일 (AES128)     |
|                | cbc128.pickle, ecb128.pickle, ofb128.pickle | AES128, 128 * 128 pixel 이미지에 대한 pickle 값 (array, dtype=uint8) |
|                |                                             |                                                              |
| AES, DES       | aes, des block enc.ipynb                    | AES128, Single DES 블록별 암호화 알고리즘 구현<br>(블록만으로 구현했기 때문에 전체 평문에 대해서는 추가 구현 필요) <br>(암호화 이미지 판별을 위한 것이기 때문에 복호화 알고리즘은 따로 구현하지 않았다.) |



<h3> 2. 실험 내용 (AES128) </h3>

- 처음에 암호화를 256 * 256 pixel에 맞춰 진행하여 AlexNet과 유사한 크기이므로 AlexNet과 유사하게 모델링을 계획 
  - 정확도가 55%를 넘지 못함 (유사 설계를 하다보니 적합하지 않은 모델링)
- kaggle의 개와 고양이 이미지 분류 모델과 유사한 모델링 계획
  - ECB, CBC 구분 성공적
  - 256 * 256 보다 128 * 128 pixel에서 더 높은 정확도를 가져 pickle 파일 128pixel에 대해 저장
- CBC, OFB 구분 가능한지 실험
  - 육안으로는 구분 불가능
  - ECB, CBC 구분과 같은 모델 적용시 구분 불가 (정확도 50% 이하)
- 라운드 별 구분 불가능 확인 
  * 라운드 별로 육안으로 판별 가능에 대해 실험한 결과, AES는 구분 불가 판정



###  3. 실험내용 (Single DES)

- DES의 경우 AES와 마찬가지로 16라운드 모두 진행 시 ECB 외에는 구분 불가

- DES의 경우 키를 활용한 연산이 라운드 함수 내에서 1번 작용하므로 라운드 별 구분이 어느 정도 가능할 것으로 판별

  * ECB 모드의 경우 3라운드에서 16라운드의 결과값과 유사함을 확인
  * 1라운드 : 암호화가 잘 이루어지지 않음, 2라운드 : 형태와 색상 유추 가능
  * CBC 모드의 경우 2라운드에서 16라운드의 결과값과 유사함을 확인
  * 1라운드 : 암호화는 어느 정도 이루어지지만, 완벽히는 이루어지지 않음
  * OFB 모드의 경우 3라운드에서 16라운드의 결과값과 유사함을 확인
  * 1라운드 : 암호화가 거의 이루어지지 않음. 2라운드 : ECB 1라운드와 유사한 정도의 암호화

  * (아직 데이터셋 및 학습 모델은 만들지 않음. 추가적인 연구 필요)