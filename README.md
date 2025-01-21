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
