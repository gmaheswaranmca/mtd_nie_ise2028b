1. Update Linux + install Git + clone repository
$ sudo dnf update -y
$ sudo dnf install git -y
$ git clone https://github.com/gmaheswaranmca/mtd_nie_ise2028b.git

2. Install Python + dependencies + start FastAPI silently
cd /home/ec2-user/mtd_nie_ise2028b/exercises/ex02/server/scripts
chmod +x ser2.sh
./ser2.sh

3. Update FRONTEND_URL and restart FastAPI
cd /home/ec2-user/mtd_nie_ise2028b/exercises/ex02/server/scripts
chmod +x ser3.sh
./ser3.sh http://13.234.100.50