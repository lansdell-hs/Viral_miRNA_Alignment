import sys
import os
import subprocess
from itertools import islice

if len(sys.argv) <2:
    print("Usage: python %s requires genomic index base name (not extension)" % sys.argv[0])
    sys.exit()

dirs= os.listdir('.')
virus=sys.argv[1]



def cutadapt_runner(file):

    name=file.split('.')[0]
    # untrimmed=name+'_untrimmed'
    # too_short=name+'_short'
    outfile= name+'_trimmed.fastq'
    cmd1=['cutadapt', '-a','ADAPTERHERE','-O','10','-m', '14','-u','0','-o',outfile, file]
    subprocess.call(cmd1)
    return outfile


def size_seperate_read_length(outfile):

    with open(outfile) as f:
        next(f)
        maxRNA= len(next(f).rstrip()) - 13  # cutadapt overlap -(14) + error (1)

    #file_di, len_tot_di = {}, {}
    #for length in range(int(14), int(maxRNA) + 1):
     #   file_di[length] = open((outfile, str(length)), 'w')
      #  len_tot_di[length] = 0
    #file_di['NP'] = open('{}_notProc.'.format(outfile), 'w')
    #len_tot_di['NP'] = 0

    tot_reads = 0
    with open(outfile) as f:
        while True:
            read = list(islice(f, 4))
            if not read:
                break
            tot_reads += 1
            read_len = len(read[1].rstrip())
            #if int(14) <= read_len <= int(maxRNA):
             #   output_di[read_len].write(''.join(read))
              #  len_tot_di[read_len] += 1
           # else:
            #    output_di['NP'].write(''.join(read))
             #   len_tot_di['NP'] += 1
              #  print('Out of size range: {}'.format(read[1].rstrip()))

    return read_len 

def bowtie_runner(outfile, read_len):

    #unaligned=outfile.split()[0]
    fin_align=outfile.split('.')[0]+'aligned.sam'
    cmd2=['bowtie','-S','-q','-nomaqround','-a','-m','20','--phred33-quals','-n','0','-e','70','-l',read_len,'--seed=197',virus,outfile,fin_align ]
    subprocess.call(cmd2)


def main():
    for f in dirs:
        if f.endswith(".fastq"):
            outfile=cutadapt_runner(f)
            read_len=size_seperate_read_length(outfile)
            bowtie_runner(outfile,str(read_len))



main()
