# 工作总结

## 1.服务器资源

**SSH禁用root登录, 请输入普通用户登录后进行`su - root`或使用`sudo`**

| Server       |                                                     |
| :----------- | :-------------------------------------------------- |
| IP           | 10.\*.\*.\*                                         |
| User         | r\*\*\*\*\*                                         |
| Password     | R\*\*\*\*\*\*\*\*\*                                 |
| RootPassword | N\*\*\*\*\*\*\*\*\*\*\*                             |
| CPU          | Intel(R) Xeon(R) Gold 5320 CPU @ 2.20GHz 8Core \* 2 |
| GPU          | Tesla A10 24G \* 3                                  |
| Memory       | 32GB                                                |
| Disk         | /: 200GB, /data1: 500GB                             |
| SSH          | \[r\*\*\*\*@\]10.\*.\*.\*                           |

## 2.显卡驱动

![4bb0fe6612b5571f3b5f59ec35bca25](./static/images/4bb0fe6612b5571f3b5f59ec35bca25.png)

## 3.CUDA驱动

![2e3e99e0a11c8689301a12e991b5f03](./static/images/2e3e99e0a11c8689301a12e991b5f03.png)

## 4.Cudnn驱动

![a9b77f4e7e6a4240f7444084553025a](./static/images/a9b77f4e7e6a4240f7444084553025a.png)

## 5.Anaconda

![767d1b8dadf145ad32c684ca299d39c](./static/images/767d1b8dadf145ad32c684ca299d39c.png)

## 6.程序运行环境

![9ddc8e1174d6c6337a1719ac2619387](./static/images/9ddc8e1174d6c6337a1719ac2619387.png)

## 7.Pytorch/Pip依赖

![845a28705ca600bb0a3cc2b5890da0a](./static/images/845a28705ca600bb0a3cc2b5890da0a.png)

## 8.制作数据集

![71091539c89e0e44fec2d90876ce1d9](./static/images/71091539c89e0e44fec2d90876ce1d9.png)

## 9.模型训练(外推模型)

![](./static/images/8fdd64c07c6b8759ad73fbf6783189f.png)

## 10.Gan(外推效果图像清晰化)数据集

![c1e482144beb3317e256e0a1c57e1d7](./static/images/c1e482144beb3317e256e0a1c57e1d7.png)

## 11.模型训练(Gan模型)

![62bbf7484b87049a082b26ac94505eb](./static/images/62bbf7484b87049a082b26ac94505eb.png)

## 12.程序运行脚本(等模型训练完成)

![e2fcb4652cb89556078d752e0b96072](./static/images/e2fcb4652cb89556078d752e0b96072.png)

```shell
#!/bin/bash
# @File: radar_management.sh
# @Editor: PyCharm
# @Author: Alice(From Chengdu.China)
# @HomePage: https://github.com/AliceEngineerPro
# @CreatedTime: 2023/2/10 19:49

# Global Variables
project_home="/home/runnet/app/radar_extrapolation_hainan/"
conda_environment_name="radar_extrapolation"

# Change Anaconda --> $(ENVIRONMENT_NAME)
environment_config() {
  if [ "$(conda --version | wc -l)" = 1 ]; then
    conda activate "$conda_environment_name"
  else
    {
      local str_a="Error: Not Found Anaconda"
      local str_b="Please install Anaconda or Add Anaconda to the environment variable (User or System)"
      printf "\033[31m%s\n \033[0m" "$str_a"
      printf "\033[31m%s\n \033[0m" "$str_b"
      exit 1
    }
  fi
}

# Change User --> root
static_user() {
  if [ "$(whoami)" = root ]; then
    environment_config
  else
    {
      printf "\033[31mInsufficient Permissions, Use root Privileges\n \033[0m"
      exit 1
    }
  fi
}

run_project() {
  # static_user
  mkdir -p "${project_home}/logs"
  local str_a="Please check program path"
  cd "${project_home}" || printf "\033[31mError: Not Found %s\n \033[0m" "${project_home}" && printf "\033[31m %s \033[0m" "$str_a"
  cp run.py radar_run.py
  nohup /data1/runnet_data/software/conda/envs/radar_extrapolation/bin/python -u radar_run.py >./logs/radar_run.log 2>&1 &
  printf "Output log file to: %s/logs/run.log\n" "$project_home"
}

start_project() {
  if [ "$(ps -ef | grep -v grep | grep radar_run | wc -l)" = 1 ]; then
    printf "Running\n"
  else
    {
      run_project
    }
  fi
}

stop_project() {
  if [ "$(ps -ef | grep -v grep | grep radar_run | wc -l)" = 1 ]; then
    kill -9  "$(ps -ef | grep -v grep | grep radar_run | awk '{print $2}')"
  else
    {
      printf "Please execute start\n"
    }
  fi
}

restart_project() {
  stop_project
  printf "wait 10s\n"
  sleep 10
  start_project
}

# main
while true; do
  case $1 in
  start)
    start_project
    break
    ;;
  stop)
    stop_project
    break
    ;;
  restart)
    restart_project
    break
    ;;
  -h | help)
    printf "
    Usage: 
          start         -Start Service
          stop          -Stop Service
          restart       -Restart Service
          -h, help      -display this help and exit\n"
    break
    ;;
  *)
    options_error="Invalid option, Please execute -h or help"
    printf "\033[31m%s\n \033[0m" "$options_error"
    break
    ;;
  esac
done

#for param in "$@"; do
#  echo "Param: $param"
#done
```

## 13.业务系统交互

> FTP方式
> 数据输入: ftp://10.\*.\*.\*/img_src
> 数据输出:ftp://10.\*.\*.\*/radar_extrapolation_hainan/predict_output

![8db4233295cf6f4d0f318b9e289bb2b](./static/images/8db4233295cf6f4d0f318b9e289bb2b.png)

