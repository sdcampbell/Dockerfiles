FROM ubuntu:latest

# Install CrackMapExec
RUN apt update && apt-get install -y libssl-dev libffi-dev python python-dev python-pip build-essential git && git clone --recursive https://github.com/byt3bl33d3r/CrackMapExec /root/CrackMapExec && cd /root/CrackMapExec && pip install -r requirements.txt && python setup.py install

# Install lsassy
RUN apt install -y python3-pip && git clone https://github.com/Hackndo/lsassy.git /root/lsassy && cd /root/lsassy && python3 setup.py install

# Install lsassy CrackMapExec module
RUN cp /root/lsassy/cme/lsassy.py /root/CrackMapExec/cme/modules/ && cd /root/CrackMapExec && python setup.py install

# Copy procdump files
COPY procdump* /opt/

CMD cme
