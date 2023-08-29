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
mkdir save
cd /workspace/MB-iSTFT-VITS2/
python train.py -c /workspace/MB-iSTFT-VITS2/filelists/config.json -m /workspace/save
