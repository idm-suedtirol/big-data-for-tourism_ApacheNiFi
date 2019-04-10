# Install Nifi (port 8080) #

*Create user to run nifi*
adduser seacom

cd /usr/share/

*Download  nifi tar.gz*
wget http://archive.apache.org/dist/nifi/1.8.0/nifi-1.8.0-bin.tar.gz

*Extract the file* 
tar -xvf /home/seacom/installazione/nifi/nifi-1.8.0-bin.tar.gz

*Copy the configuration to run_as*
cp /home/seacom/installazione/nifi/bootstrap.conf /usr/share/nifi-1.7.1/conf/boostrap.conf

*Change file and folder ownership*
chown -R seacom:seacom nifi-1.8.0

cd /usr/share/nifi-1.8.0/bin

*Install nifi as service*
./nifi.sh install

*Enable service on boot*
systemctl enable nifi

*Start the service*
systemctl start nifi

*Create the ingestion folder*
cd /home/seacom
mkdir data 
cd data 
mkdir IDM
cd IDM
mkdir new
mkdir delete
mkdir processed
chown -R seacom:seacom /home/seacom/data/ 

*Create the folder wirh the data for the enrichemnt*
mkdir /home/seacom/lookup
cp /home/seacom/installazione/nifi/country-code.csv /home/seacom/lookup/ 
cp /home/seacom/installazione/nifi/municipalities-latlon.csv /home/seacom/lookup/ 
chown -R seacom:seacom /home/seacom/lookup/ 

# Create the ingest flow #

Open the nifi interface at http://<ip>:8080/nifi

*Upload the template*
(“operate” window, 7th button from the left)

*Deploy a new flow from the template*
(create from template button, main window 7th button from the left)

*Configure the attributes for the flow*
Right click the group, select variables, set the variable value
- es_url (elasticsearch url) http://<ip_elastic>:9200/
- es_user (elasticsearch user) seacom
- es_password (elasticsearch password) *******
- enrich path /home/seacom/lookup
- folder_path (base path for the ingestion)  /home/seacom/data/IDM

*Start the flow*
Right click on the flow group select start