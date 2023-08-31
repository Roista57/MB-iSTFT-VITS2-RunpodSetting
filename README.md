# MB-iSTFT-VITS2-RunpodSetting
```
apt update
apt install cmake unzip zip ffmpeg espeak python3.8 python3.8-venv python3.8-dev portaudio19-dev -y
update-alternatives --install /usr/bin/python python /usr/bin/python3.8 1
update-alternatives --config python
python3 --version
wget https://bootstrap.pypa.io/get-pip.py
python3.8 get-pip.py

git clone https://github.com/FENRlR/MB-iSTFT-VITS2.git
pip install torch==1.13.1 torchvision==0.14.1 torchaudio==0.13.1 --index-url https://download.pytorch.org/whl/cu117
cd MB-iSTFT-VITS2
# requirements.txt에서 pyopenjtalk을 삭제해주세요.
pip install -r requirements.txt
pip install -U pyopenjtalk==0.2.0 --no-build-isolation

cd monotonic_align
rm -rf ./monotonic_align
mkdir monotonic_align
python setup.py build_ext --inplace
cd ..

cd /workspace/
mkdir model
cd /workspace/MB-iSTFT-VITS2/
python train.py -c /workspace/MB-iSTFT-VITS2/filelists/config.json -m /workspace/model
```
# Tensorboard terminal
## 가상환경 설정
```
python -m venv venv
source venv/bin/activate
pip install ipykernel
pip install tensorboard
python -m ipykernel install --user --name venv --display-name tensorboard
```
# Tensorboard Jupyter notebook(.ipynb)
## !중요! 주피터 노트북의 커널을 tensorboard로 변경해주어야 한다.
### (tensorboard 커널이 생성되는데 최대 5분정도 걸릴 수 있음)
## 텐서보드 열기
```
%load_ext tensorboard
%tensorboard --logdir="/workspace/model" --port 6006
```
## ngrok 설치
```
!rm -f ngrok
!wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
!unzip -o ngrok-stable-linux-amd64.zip
```
## 토큰 설정(회원 가입 필요)
### https://dashboard.ngrok.com/get-started/your-authtoken
### copy 버튼을 누르고 your_token = "붙여넣기"
```
your_token = ""
!./ngrok authtoken {your_token}
```
## ngrok 백그라운드 실행
```
get_ipython().system_raw('./ngrok http 6006 &')
```
## 실행중인 ngrok URL 확인
```
import json
import urllib.request

ngrok_info = urllib.request.urlopen('http://localhost:4040/api/tunnels')
tunnels = json.load(ngrok_info)
tunnel_url = tunnels['tunnels'][0]['public_url']
print(tunnel_url)
```
