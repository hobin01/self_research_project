서울시립대학교 대학 혁신 지원 사업 - 학생 미래 설계 학기 자기주도 연구 프로젝트 

주제 : 딥러닝을 활용한 암호 알고리즘 분석

추가 설명 : https://ghqls0210.tistory.com/category/%EC%95%94%ED%98%B8%ED%95%99/%EC%9E%90%EA%B8%B0%EC%A3%BC%EB%8F%84%EC%97%B0%EA%B5%AC%20%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8



#### 실행 환경 : Google Colab

#### API : tensorflow2 



<h3> 1. 데이터셋 모으기</h3>

    - data_making
    crawling.ipynb : 데이터셋을 위한 이미지 크롤링
    
    img_data_making.ipynb : 크롤링한 자료 type, shape 일치시키기
    
    img_encrypt.ipynb : 이미지 데이터 암호화 (AES128, ECB, CBC, OFB 모드) (1000장)
    
    AES128Encrypted10roundImageData.zip : AES128을 이용해 암호화된 이미지 압축 파일 (256*256 pixel)
    
    - 128pixel_model
    128 modle test.ipynb : 128 픽셀 ECB, CBC, OFB 데이터 모델 및 학습 파일
    
    pickles128pixel.zip : 128 * 128 pixel 이미지에 대한 pickle 값 (array, dtype=uint8)



<h3> 2. 실험 내용 </h3>

- 처음에 암호화를 256 * 256 pixel에 맞춰 진행하여 AlexNet과 유사한 크기이므로 AlexNet과 유사하게 모델링을 계획 
  - 정확도가 55%를 넘지 못함 (유사 설계를 하다보니 적합하지 않은 모델링)
- kaggle의 개와 고양이 이미지 분류 모델과 유사한 모델링 계획
  - ECB, CBC 구분 성공적
  - 256 * 256 보다 128 * 128 pixel에서 더 높은 정확도를 가져 pickle 파일 128pixel에 대해 저장
- CBC, OFB 구분 가능한지 실험
  - 육안으로는 구분 불가능
  - ECB, CBC 구분과 같은 모델 적용시 구분 불가 (정확도 50% 이하)