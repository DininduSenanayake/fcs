# Fetch the modified `fcs.py` script 

```bash
curl -LO https://raw.githubusercontent.com/DininduSenanayake/fcs/refs/heads/main/dist/fcs.py
```

# Method 1 :  Copy the database from file system to memory during runtime 

* 


```bash
#!/bin/bash -e

#SBATCH --job-name      fcs-gx
#SBATCH --time          24:00:00
#SBATCH --partition     hugemem
#SBATCH --mem           500G                           #need a minimum of 470G to coy the database to memory and some for computation
#SBATCH --cpus-per-task 24
#SBATCH --output        slog/%j.out


module purge >/dev/null 2>&1 
module load Apptainer/1.2.5 Python/3.8.2-gimkl-2020a

# Copy the database from filesystem to memory 
mkdir $TMPDIR/$USER-FCSDB
cp /opt/nesi/db/FCS-GX/2024-04/* $TMPDIR/$USER-FCSDB
export FCSDB=$TMPDIR/$USER-FCSDB

export FCS_DEFAULT_IMAGE=/opt/nesi/containers/fcs/fcs-gx-0.5.5.sif

#create the output directory
mkdir -p  my_output


python3 ./fcs.py --container_platform apptainer  screen genome --fasta my.fa  --out-dir my_output  --gx-db $FCSDB --tax-id 123456
```

* If you would liike to use `milan` patition for this, make sure to include following variable

```bash
#SBATCH -w wml[061-068]
```