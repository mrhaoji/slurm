#Install
tar -xf slurm-2.5.3.tar.bz2
./configure --prefix=/home/hchen71/slurm --sysconfdir=/home/hchen71/slurm/etc
make
make install

#Configure generating
cp etc/slurm.conf.example /etc/slurm.conf

#Configuration for torque
cd etc/
openssl genrsa -out slurm.key 1024
openssl rsa -in slurm.key -pubout -out slurm.cert

#Run
cd slurm/sbin
./slurmctld -Dc
./slurmd -Dc

cd slurm/bin
./sinfo

./scontrol show partition
./scontrol show node node-name

./srun -N1 hostname
./srun -N1 sleep 30 &

./squeue

./sbatch /script/to/run
bash /script/to/run > /output_.txt

./scancel JOBID

ps -A | grep slurm
pkill slurmd
pkill slurmctld 

awk '{sum=sum+$1}{total=sum*0.000001}{jobs=100}{tp=jobs/total}END{printf("%f sec\n %f jobs/sec\n",total,tp) }' ../log/o.txt >> ../log/r.txt

#References
[SLURM Version 2.3 Configuration Tool](https://computing.llnl.gov/linux/slurm/configurator.html)   
[slurm的安裝](http://blog.sina.com.cn/s/blog_61b8694b0100s6bf.html)   
[用 SLURM 优化超级计算机内的资源管理](http://www.ibm.com/developerworks/cn/linux/l-slurm-utility/)   
[Slurm Quick Start Tutorial](http://www.ceci-hpc.be/slurm_tutorial.html)