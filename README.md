## Download Assemblies Accessions

Here I am showing how to download all the assemblies from your search result using the assembly accessions.

First, search the assemblies you want in NCBI (https://www.ncbi.nlm.nih.gov/assembly)

In this example, we are searching Pyricularia assemblies using it's taxid "txid48558" in the search box.

You should get result as shown here: [NCBI Pyricularia Result] (https://www.ncbi.nlm.nih.gov/assembly/?term=txid48558%5bOrganism:exp)

In order to get the accessions of the assemblies in the result:
1) click "send to" link
2) select "file" option
3) Under format option, select "ID Table (text)"
4) Under sort by option, select "Accession"

See the screenshot below:

![Download assembly accessions] (download_assembly_accessions.png)

5) click "Create File"

This will download a file. Save the file with a filename as you wish.

## Clean the file to get accessions

The downloaded file has multicolumns with accessions in the first column. Remove the header in the first line and the other columns. This can be best done with the following command in the command line:

```
tail -n+2  downloadedfile.txt | awk '{print $1}' > accessions.txt
```

We have the accessions in accessions.txt or any filename you give in the command above.

## Download the assemblies using the accessions

There is a simple way to download the assemblies using wget option:

```
while read gca; do first=${gca:4:3}; second=${gca:7:3}; third=${gca:10:3};  wget -r -nd -nc -A "${gca}_*genomic.fna.gz" -R "*cds_from*" -R "*_rna_from_*" ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/$first/${second}/${third} ; done < accessions.txt
```

### Command explanation:

1) While loop read each accession from accessions.txt file one at a time. 
2) In each accession, we are splitting the numbers by three digits. E.g. In accession GCA_000292585.1, we are splitting the number 000292585 to 000, 292 and 585. This is because the assemblies are stored in NCBI FTP server and the folder structures are named by 3 digits in the accessions. For this accession, the ftp link is: ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/000/292/585
3) The ftp link is constructed by using the variables first, second and third and supplied in the wget command
4) wget command options: 
	- r will look for the files in every folder in the ftp path
	- nd will save the files in the current working directory (not the same folder structure as in the ftp server)
	- nc will prevent downloading the files, if they are already downloaded
	- A will accept any file with the given pattern for downloading
	- R will reject any file with the given pattern for downloading




