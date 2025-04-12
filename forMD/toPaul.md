# Deep learning research topic
- Jinhui Li, 04102025, SLU.
- This document is used to guide how to generate the data for the deep learning research.
- The data contain two types, Pssm and contact matrices.
- The document format of this document is markdown, which can be convert to the webpage. Here is a guide, you can study if interested in: https://www.markdownguide.org/getting-started/
## Method details
### 1. prepare a clean folder in server.
- Login your account in our server by Mobaxterm. 
- CP(Copy and paste) bellow command at the terminal.
```
mkdir -p pssm_cmt # all your operate should be in this folder.
cd pssm_cmt
mkdir -p cmt fa pssm  pdb json  
```
### 2. Upload all alignment files and code files into pssm_cmt folder.
- Go to the folder created in last step in Mobaxterm.
  
![](https://raw.githubusercontent.com/jinhuili-lab/personal_image_bed/master/img2025/202504112340368.png)
- Drag the file to the area in the picture.
  
![](https://raw.githubusercontent.com/jinhuili-lab/personal_image_bed/master/img2025/202504112342441.png)
### 3. Convert alignment(.aln) to fasta(.fa) file.
- Copy all the bellow content  and  paste at the command line from local folder, press enter.
```
python3 aln2fa.py  *aln  # generate fa files from aln files.
python3 fa2json4af.py  *.fa  # generate jsons  for alphafold server from fa files.
python3 mergejs.py # merge all json files into  all.json. upload all.json on alphafold server. When you finish all, download all.
```
- Check the fa files, json files.
![](https://raw.githubusercontent.com/jinhuili-lab/personal_image_bed/master/img2025/202504112348385.png)
### 4. Generate the PSSM files.
- Copy the bellow command and paste on the terminal.  press enter button double time.
![](https://raw.githubusercontent.com/jinhuili-lab/personal_image_bed/master/img2025/202504112351814.png)
```
for i in `ls  *.fa`; do
echo "psiblast -query $i  -db nr -evalue 0.001 -save_pssm_after_last_round   -out_ascii_pssm   $i.ascii_mtx_file.pssm   -num_threads  30  -num_iterations 2 " >>fa2pssm.sh ;
done
nohup bash fa2pssm.sh  & # generate pssm
```
- Here the task has been submited. PSSM is generating now, you can use  "htop" command to check the process, and "q" to quit the htop view. 
- Each pssm file will spend 4 minutes.
![](https://raw.githubusercontent.com/jinhuili-lab/personal_image_bed/master/img2025/202504112354326.png)
- When all are finished, there will be not a psiblast task. 
- At the same time, we can try to generate the contact matrix.
### 5. Generate the contact matrix files.
- Download the "all.json" to your local folder(drag it to the local folder).
![](https://raw.githubusercontent.com/jinhuili-lab/personal_image_bed/master/img2025/202504120001133.png)
- login the alphafold server website. If no account, register it firstly.
> https://alphafoldserver.com/
- Each account can submit 30 jobs and create 100 drafts every day.
![](https://raw.githubusercontent.com/jinhuili-lab/personal_image_bed/master/img2025/202504120007965.png)
- Click "Upload JSON" button, upload the file "all.json" and submit them as draft.
![](https://raw.githubusercontent.com/jinhuili-lab/personal_image_bed/master/img2025/202504120013559.png)
- Use the filter "draft" to view all the draft.
![](https://raw.githubusercontent.com/jinhuili-lab/personal_image_bed/master/img2025/202504120015168.png)
- Submit the draft separately.
- Click one draft, and the sequence box will be update. Click "continue and preview"
![](https://raw.githubusercontent.com/jinhuili-lab/personal_image_bed/master/img2025/202504120018646.png)
- "Confirm and submit." and go to the process filter to check the process.
![](https://raw.githubusercontent.com/jinhuili-lab/personal_image_bed/master/img2025/202504120019137.png)
- When all are finished, download all.
![](https://raw.githubusercontent.com/jinhuili-lab/personal_image_bed/master/img2025/202504120020801.png)
- Extract all files from the conpressed file downloaded.
- Copy all the "_model_0.cif" files from every folder to one position. 
- Rename them with a uniform format. eg: Ntox29.WP_100211745.pdb. Ntox29 is from the aln file. WP_100211745 is the ID from the fa file. remove other element of the file name.
- Drag/Upload them to our server.
![](https://raw.githubusercontent.com/jinhuili-lab/personal_image_bed/master/img2025/202504120027725.png)
![](https://raw.githubusercontent.com/jinhuili-lab/personal_image_bed/master/img2025/202504120032368.png)
- convert cif files to pdb files. copy paste bellow command.
```
pip3 install biopython # Install biopython
python3 cif2pdb.py *cif # convert cif to pdb.
python3 pdb2contactmatrix.py  *pdb # Generate contact matrix.
```
- Now, all the contact matrics are generated.
### 6. Data clean
- When PSSM generating is finished, try to clean the folder on our server. 
- copy and paste the bellow command. Wait. ensure your working direction is in pssm_cmt/
```
mv *.pssm pssm
mv *.pdb  pdb
mv *.cif pdb
mv *.json json
mv *.T.cm.txt cmt
mv *.fa fa
```
### 7. Finished.
- Download/Drag the pssm_cmt folder to your local folder.
- Package them and compress them. 
