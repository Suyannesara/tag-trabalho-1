### Como instalar e rodar tudo:
cd projeto-grafos

sudo apt update
sudo apt install -y python3-full python3-venv git

python3 -m venv .venv
source .venv/bin/activate

python -m pip install -r requirements.txt

jupyter notebook