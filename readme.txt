Testing Staking DApp :
Open this link and connect your Metamask wallet.
Link Here :- https://test-staking.nulink.org/


2. This will switch Metamask network to Binance Testnet. Now, go to the Faucet tab and click on Get the BNB: Binance Smart Chain Faucet. Enter your address and click on Give Me BNB >> 0.5 BNBs



3. Now click on the big purple Faucet button and approve the transaction in Metamask. After the transaction is confirmed, you’ll have 50000 NLK.




4. Now, go to this link and click on Staking.

Link Here :- https://test-staking.nulink.org/


5. Enter 50000 and click on Confirm. Approve the transaction in your Metamask wallet.


6. It will look something like this :


7. That’s all for this. Let’s head over to setting NuLink Worker.

Setting Up NuLink Worker :
There are four steps to run a NuLink Worker:

Create Worker Account
Install NuLink Worker
Configure and Run a Worker node
Bond the Worker node with your staking account
But before all this, you’ll need a VPS to setup NuLink Worker. Let’s buy it.

1. Purchase VPS
I will suggest you to order your VPS on Contabo

Link :- https://contabo.com/

Choose Most Popular Plan



Options to choose :- 1 Month >> European >> 400 GB >> Ubuntu 20.04 >> Generate Password >> Next


After it click on Next


And Now enter your details like name address and all!

Click Next and Now complete the transaction.

Once you have complete your order. You will receive a first email.


After it you will receive a mail which contain your IP.

For me its takes approx 6 hours

And right now when I am writing this Contabo is down !!

2. Connect Your VPS
For Windows :-

Now we have to connect our IP , for this we use Putty.

Link :- https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe

Now,

Paste server IP to “Host Name” and click “Open”
In the opened tab, write the command: root;
Press “Enter” and paste the password from the server, then “Enter”


Note :- Password will never we displayed , Just type and press “Enter “

For MacOS :-

On macOS, launch the Terminal.


Enter the server with the command (change IP_ADDRESS to the server IP):ssh root@IP_ADDRESS
Next, enter “yes”, press “Enter,” and paste the server password (the icon with the key will hide the entered password). Press “Enter”.
You can use a VPS that you already have also. Make sure you are not running any other node on that or these two might clash and both may fail to stay awake.

3. Installing NuLink Worker :
Let’s Update The Packages :

Copy the commands that are in the grey box and paste them in Putty your whatever method you are using to connect to your VPS.

sudo apt update && sudo apt upgrade -y

apt install python3-pip -y

Now, let’s create a worker account. Download Geth by running the following command.

wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.10.23-d901d853.tar.gz

Unzip it.

tar -xvzf geth-linux-amd64-1.10.23-d901d853.tar.gz

Enter the new folder.

cd geth-linux-amd64-1.10.23-d901d853
Generate Ethereum account and keystore by running the following command :

./geth account new — keystore ./keystore
Enter a new password and press Enter. For Example :

Aniket_1999
Confirm your password and press Enter.

Aniket_1999

Now, Save all these Information in Notepad. This will contain public address of key and path of secret key file.


Now, open your Metamask wallet and send BNB test tokens to the public address you received above. This address will also be called operator-address. The highlighted address in above screenshot is my operator-address.


Enter your operator-address in here. Enter amount as 0.2 and click Next. Confirm the transaction and return back to Putty.


Enter this command to go to $home

cd $home

Install docker on your vps

snap install docker

Pull the latest NuLink image by running this :

docker pull nulink/nulink:latest

Let the command run. Then, create a directory in your host machine for later usage.

cd /root
mkdir nulink

Let’s copy the key file to this newly created directory. Run the following command :

cp xxxxxxxxxxxxxxxxx /root/nulink
Replace xxxxxxxxxxxxxxxxx with the key path that you copied in notepad above. The key path will look like this


For Example :

cp /root/.ethereum/keystore/UTC--2022-10-09T19-58-37.046465423Z--b8d1ffd8aeee04f05bf908b1adfb1af00fcb1b97 /root/nulink

Let’s give this directory 777 permissions. 777 permission means every user can Read, Write, and Execute. Run the below code.

chmod -R 777 /root/nulink

In order to isolate global system dependencies from nulink-specific dependencies, we highly recommend using python-virtualenv to install nulink inside a dedicated virtual environment.

pip install virtualenv

Create a virtual environment in a folder somewhere on your machine. This virtual environment is a self-contained directory tree that will contain a python installation for a particular version of Python, and various installed packages needed to run the node.

virtualenv /root/nulink-venv

Activate the newly created virtual environment by running these :

source /root/nulink-venv/bin/activate

Download the Nulink package :

wget https://download.nulink.org/release/core/nulink-0.2.0-py3-none-any.whl --no-check-certificate

Install the NuLink Package :

pip install nulink-0.2.0-py3-none-any.whl

Before continuing, verify that your Nulink installation and entry points are functional.

Activate your virtual environment, if not activated already:

source /root/nulink-venv/bin/activate

Now, verify NuLink is importable. If there’s no response, that means it’s successful.

python -c "import nulink"

Then, run this :

nulink --help

Now let’s initialize NuLink Worker and Run it. Export these environment variables. These environment variables are used to better simplify the Docker installation process.

export NULINK_KEYSTORE_PASSWORD=xxxxxxxxx
Replace xxxxxxxxx by a new password. Minimum 8 Characters needed. Don’t forget it. For Example :

export NULINK_KEYSTORE_PASSWORD=Aniket_1999
Now, enter this :

export NULINK_OPERATOR_ETH_PASSWORD=xxxxxxxxx
Replace xxxxxxxxx by the password you just entered above. For Example :

export NULINK_OPERATOR_ETH_PASSWORD=Aniket_1999

Let’s run node via Docker. Run this.

docker run -it --rm \
-p 9151:9151 \
-v /root/nulink:/code \
-v /root/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
nulink/nulink nulink ursula init \
--signer keystore:///code/xxxxxxxxxxxxxx \
--eth-provider https://data-seed-prebsc-2-s2.binance.org:8545/ \
--network horus \
--payment-provider https://data-seed-prebsc-2-s2.binance.org:8545/ \
--payment-network bsc_testnet \
--operator-address yyyyyyyyyyyyy \
--max-gas-price 100
Replace xxxxxxxxxxxxxx with the path to the key store file of the Worker account


Replace yyyyyyyyyyyyy with operator address.


For Example :

docker run -it --rm \
-p 9151:9151 \
-v /root/nulink:/code \
-v /root/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
nulink/nulink nulink ursula init \
--signer keystore:///code/UTC--2022-10-09T19-58-37.046465423Z--b8d1ffd8aeee04f05bf908b1adfb1af00fcb1b97 \
--eth-provider https://data-seed-prebsc-2-s2.binance.org:8545/ \
--network horus \
--payment-provider https://data-seed-prebsc-2-s2.binance.org:8545/ \
--payment-network bsc_testnet \
--operator-address 0xb8D1fFD8aEeE04F05bf908B1adfB1aF00FCB1b97 \
--max-gas-price 100
Now, press y and Enter.


Take a screenshot of your seedphrase or copy it somewhere safe. Press y then Enter.


Now, write down your seedphrase that you copied/screenshotted above. I will suggest you to write it down in notepad and make sure there are no errors before copying it and pasting it in terminal.


Now, press Enter and wait for some time.


There will be bunch of info that has been given on your terminal. Make sure to save it in a notepad.

We are all set. Let’s launch our Node.

docker run --restart on-failure -d \
--name ursula \
-p 9151:9151 \
-v /root/nulink:/code \
-v /root/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
-e NULINK_OPERATOR_ETH_PASSWORD \
nulink/nulink nulink ursula run --no-block-until-ready

The following command describes how to view worker addresses.

docker logs -f ursula

Press CTRL + Z to stop the log.

Next, go to $home by running the following command.

cd $home
Use the commands below to open necessary ports :

apt install ufw -y 
ufw allow ssh 
ufw allow https 
ufw allow http 
ufw allow 9151
ufw enable
Press Enter


Now, press y and Enter.

You can close Putty Now.

Now, Click Here.

Click on Bond Worker.

Enter your Worker Address. Your worker address is your operator-address. For Example :


NODE URI= https://Your_adress_VPS:9151

For Example :

NODE URI=https://4.233.139.131:9151

Enter both and click on confirm.


Now, approve transaction in Metamask.


Congratulations. You have successfully installed and are running NuLink Worker.


Also, Don’t forget to fill the form : https://forms.gle/MBzxNbJ57pEd3hh27

Chill and thanks

Help :- If you are having some serious issue then u can join our telegram group for help and keep in mind we will reply in sometime.

Group — https://t.me/+_dqEiYHP7WY5N2Nl

Guys don’t forget to clap max on this tutorial pls it takes hardwork.

Thanks its done for today.
Don’t forget to follow admin and join telegram for fastest update. lol

Link :- telegram.me/Blokhash
Link:- twitter.com/dude_its_ritik

I read every comment on our post , so don’t forget to comment your views on our work.
