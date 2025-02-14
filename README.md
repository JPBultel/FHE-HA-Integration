### Environment Preparation:
Instructions [here](ENVIRONMENT.md)

### 1. Connect to the Encrypt-HAvm:
```
ssh -i ~/.ssh/Encrypt_HAtestVM_keys.pem azureuser@40.68.227.213
```
You nedd 2 terminals :
- Terminal 1 is for manipulating containers. 
- Terminal 2 is for data sharing.

### Clone the repository:
*(in Terminal 1)*
```
git clone git@github.com:JPBultel/FHE-HA-Integration.git
```

Change working directory in *BOTH* terminals to :
```
cd ~/FHE-HA-Integration/
```

### 2. Generate keys, encrypt client input data and a binary decision tree (The clear data is already in HEHAEnc/data):

#### 2.1 Build the tool:
*(in Terminal 1)* 
```
cd HEHAEnc &&\
docker build -t heha-enc .
```
#### 2.2 Run the tool:
*(in Terminal 1)*
```
docker run -it --name heha-enc heha-enc &&\
./encrypt-fintech-setup
```

#### 2.3 Share the keys and the encrypted data:
*(in Terminal 2)*
```
cd HEHAMain/build/data &&\
docker cp heha-enc:/bdt/build/results/cryptocontext.txt . &&\
docker cp heha-enc:/bdt/build/results/key-public.txt . &&\
docker cp heha-enc:/bdt/build/results/key-eval-mult.txt . &&\
docker cp heha-enc:/bdt/build/results/encrypted_data0.txt . &&\
docker cp heha-enc:/bdt/build/results/encrypted_data1.txt . &&\
docker cp heha-enc:/bdt/build/results/encrypted_data2.txt . &&\
docker cp heha-enc:/bdt/build/results/encrypted_data3.txt . &&\
docker cp heha-enc:/bdt/build/results/encrypted_data4.txt . &&\
docker cp heha-enc:/bdt/build/results/encrypted_data5.txt . &&\
docker cp heha-enc:/bdt/build/results/encrypted_data6.txt . &&\
docker cp heha-enc:/bdt/build/results/encrypted_tree0.txt . &&\
docker cp heha-enc:/bdt/build/results/encrypted_tree1.txt . &&\
docker cp heha-enc:/bdt/build/results/encrypted_tree2.txt . &&\
docker cp heha-enc:/bdt/build/results/encrypted_tree3.txt . &&\
docker cp heha-enc:/bdt/build/results/encrypted_tree4.txt . &&\
docker cp heha-enc:/bdt/build/results/encrypted_tree5.txt . &&\
docker cp heha-enc:/bdt/build/results/encrypted_tree6.txt . &&\
cd ../../../HEHADec/build/data &&\
docker cp heha-enc:/bdt/build/results/cryptocontext.txt . &&\
docker cp heha-enc:/bdt/build/results/key-public.txt . &&\
```

#### 2.4 Exit:
*(in Terminal 1)*
ctrl-D

************************************************************************************
### 3. Perform the homomorphic evaluation:
************************************************************************************

#### 3.1 Build the tool:
*(in Terminal 1)*
```
cd ../HEHAMain &&\
docker build -t heha-main .
```

#### 3.2 Run the tool:
*(in Terminal 1)*
```
docker run -it --name heha-main heha-main &&\
./encrypt-fintech-analytics
```

#### 3.3 Share the encrypted result:
*(in Terminal 2)*
```
docker cp heha-main:/bdt/build/results/output_ciphertext.txt .
```
#### 3.4 Exit:
*(in Terminal 1)*

ctrl-D

************************************************************************************
### 4 Decrypt the result:
************************************************************************************

#### 4.1 Build the tool:
*(in Terminal 1)*
```
cd ../HEHADec &&\
docker build -t heha-dec .
```

#### 4.2 Run the tool:
*(in Terminal 1)*
```
docker run -it --name heha-dec heha-dec &&\
./encrypt-fintech-getresult
```


#### Old, Notes:

The code has been updated. Now the binary files produced are supposed to be executed without argument: ./name_of_the_file

There are 4 folders. 
- HEHAEnc is a setup/encrytion tool.
- HEHADec is a decryption tool.
- HEHAMain is the analytic component that works on encrypted data.
- accfhe-fintech is a pre-filled integration environment.

The clear inputs (binary decision tree and data tree) are now txt files inside HEHAEnc/build/data (instead of being hard-coded like in the previous version of the code).

TODO
1) Update the CUDA parts of the dockerfiles in HEHAEnc, HEHAMain, HEHADec and accfhe-fintech/Docker for compliance with the ENCRYPT platform.
2) Build and run a container from HEHAMain (docker build (...), then docker run -it (...)).
3) Extract the binary file encrypt-fintech-analytics (generated inside this container) and copy it in accfhe-fintech/app.
4) Push the folder accfhe-fintech to the ENCRYPT platform.
