FROM ubuntu:latest

RUN apt update && apt-get install -y libssl-dev libffi-dev python python-dev python-pip build-essential git && git clone --recursive https://github.com/byt3bl33d3r/CrackMapExec /root/CrackMapExec && cd /root/CrackMapExec && pip install -r requirements.txt && python setup.py install

CMD cme
