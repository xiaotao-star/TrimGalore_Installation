# TrimGalore安装

## 1 创建conda环境

进入TrimGalore目录中，安装Archiconda3并且创建环境TrimGalore

```
步骤一：安装Archiconda3
./Archiconda3-0.2.3-Linux-aarch64.sh 
按照相关指令安装即可
```

## 2 导入TrimGalore依赖环节

执行以下命令，直接导入依赖环境，其中需要修改**environment.yml**(TrimGalore目录下)中

**最后一行prefix: /path/to/archiconda3/envs/TrimGalore为自己的实际目录**。

```
conda env create -f environment.yml

激活环境
conda activate TrimGalore
```

## 3 安装fastqc中的java环境

安装JDK8以上，在TrimGalore目录中，解压并且配置java环境

```
tar -xvf zulu8.76.0.17-ca-jdk8.0.402-linux_aarch64.tar.g

##在~/.bashrc中添加java环境，其中/path/to/为自己目录的实际环境。
export PATH=/path/to/zulu8.76.0.17-ca-jdk8.0.402-linux_aarch64/bin:$PATH

##测试java
java -version
```

## 4 测试TrimGalore环境

执行以下命令测试TrimGalore中cutadapt和fastqc环境是否正常。

```
cutadapt --version
fastqc --version
```

## 5 安装TrimGalore

```
curl -fsSL https://github.com/FelixKrueger/TrimGalore/archive/0.6.10.tar.gz -o trim_galore.tar.gz
tar xvzf trim_galore.tar.gz

##配置环境并测试，其中/path/to/为自己目录的实际环境。
export PATH=/path/to/TrimGalore-0.6.10:$PATH
trim_galore --version
```

## 6 运行具体实例

进入TrimGalore的测试数据目录中，进行相关数据测试。

```
cd TrimGalore-0.6.10/test_files/

##解压fastq文件
gunzip clock_10K_R1.fastq.gz
gunzip clock_10K_R2.fastq.gz

##fastqc质控
fastqc -t 2 -o ./ clock_10K_R1.fastq clock_10K_R2.fastq

##TrimGalore过滤
trim_galore -q 20 --phred33 --fastqc --stringency 3 --length 20 -e 0.1 --paired --dont_gzip --clip_R1 4 --clip_R2 4 --illumina -o ./ ./clock_10K_R1.fastq  ./clock_10K_R2.fastq

##运行结束之后，会在当前目录下生成相关结果文件。
```

