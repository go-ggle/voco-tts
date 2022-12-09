# voco-tts
직접 녹음한 데이터셋으로 파인튜닝한 모델 데모는 [여기](https://colab.research.google.com/drive/10II3ngVZef1PdmnKU0Y_tFE8EMwFU06x)에서 확인할 수 있습니다

모델 트레이닝 순서는 아래와 같습니다. 
보다 높은 정확도를 위해 직접 녹음한 데이터셋 대신 LJ Speech Dataset을 이용해 훈련을 진행하겠습니다. 

1. 레포지토리 클론
```
git clone https://github.com/go-ggle/voco-tts.git
```
2. Synthesizer 설치
```
apt-get install espeak
```
3. 패키지 설치
```
pip install -r requirements.txt
```
4. Monotonic Alignment Search 세팅
```
cd monotonic_align
python setup.py build_ext --inplace
```

```
cd monotonic_align
mkdir monotonic_align
python setup.py build_ext --inplace
```
5. LJ Speech 데이터셋 다운로드 
```
wget https://data.keithito.com/data/speech/LJSpeech-1.1.tar.bz2
tar -xvf LJSpeech-1.1.tar.gz
```
6. 파일 경로 변경
```
sed -i -- 's,DUMMY,LJSpeech-1.1/wavs,g' filelists/*.txt
```
7. 텍스트 파일 길이 수정
```
vi filelists/ljs_audio_text_train_filelist.txt.cleaned
:500,$d
```
```
vi filelists/ljs_audio_text_val_filelist.txt.cleaned
:150,$d
```
8. config 파일 다운로드
```
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1gTp1FXfkiLVlpKh2Vg5FygaCKbsQ97FG' -O configs/train_base.json
```
7. 모델 훈련
```
python train_ms.py -c configs/train_base.json -m train_base
```
