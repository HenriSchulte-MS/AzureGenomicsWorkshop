# Exercise 1: Provision a Data Science Virtual Machine

  - [Exercise 1: Provision a Data Science Virtual Machine](#exercise-1-provision-datascience-vm)
    - [Task 1: Create a VM](#task-1)
    
    - [Task 2: Accessing the Ubuntu Data Science Virtual Machine - Graphical session](#task-2)

    - [Task 3: JupyterHub and JupyterLab](#task-3)

    - [Task 4: Testing a bioinformatics workflow](#task-4)



### Task 1: Create a VM

1. In the [Azure portal](https://portal.azure.com/), select **+Create a resource** from the menu.

    ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](img/create-a-resource.png "Create a resource")
2. In Azure Marketplace, search for "Data Science Virtual Machine - Ubuntu 18.04" or select AI + Machine Learning and click Learn More on the Data Science Virtual Machine - Ubuntu 18.04 

    ![Marketplace](img/marketplace.PNG "Marketplace")

3. Select **Create**.

    ![Data Science Virtual Machine - Ubuntu 18.04.](img/create-vm.PNG "Data Science Virtual Machine - Ubuntu 18.04.")

4. On the Create a virtual machine Basics tab, enter the following:

    - **Project details**
        - **Subscription**: Select the subscription you are using for this hands-on lab.
        - **Resource group**: Select Create new and enter "hands-on-lab" for the resource group name.

    - **Instance details**
        - **Virtual machine name**: Enter "DataScience-Ubuntu18.04-VM".
        - **Region**: Select the region you are using for resources in this hands-on lab.
        - **Availability options**: Set to no infrastructure redundancy required.
        - **Image**: Leave the Data Science Virtual Machine - Ubuntu 18.04 - Gen 1 selected.
        - **Size**: Select the size Standard_D8s_v3. You can resize the VM after creation at any time depending on your requirements (scaling up or down).

    - **Administrator account**
        - **Authentication type**: Select **Password**.
        - **Username**: labuser
        - **Password**: _Enter a valid password that you will remember_.

    ![The values specified above are entered into the Create a virtual machine Basics tab.](img/basics.PNG "Create a virtual machine Basics tab")

5. Select Disks, Networking, Management tabs to see what are the extra options for the VM, but no modifications are required.

6. Select **Review + create**. Note, the default values are used for the remaining tabs, so you don't need to set anything on those.

7. On the Review + create tab, ensure a Validation passed message is displayed and then select **Create**.

    ![Create a virtual machine Review + create tab](img/validation.png "Create a virtual machine Review + create tab")

8. It will take about 3-5 minutes to deploy the VM.

9. Once the deployment is finished, click "Go to resource" to navigate to the blade for the VM in the Azure Portal.

    ![Go to resource.](img/created-vm.PNG "Go to resource")

10. In the control bar, select **Connect**, then **SSH**.

    ![Under Connect, the command to connect to the VM displays.](img/ssh.PNG "SSH command line")

11. A dialog will appear showing the SSH command line to use to connect to the VM. It includes the VM username (labuser in the below) and IP address  used to access the VM.

12. Open Cloud Shell in Azure Portal

     ![Cloud Shell](img/shell.PNG "Cloud Shell")

13. Select Bash.

     ![Cloud Shell](img/bash.PNG "Bash")

14. Execute the modified SSH command (e.g. `ssh labuser@20.67.86.249` with your VM's IP address) to connect to the VM. Enter your VM password when prompted.

    ![An SSH shell prompt window displays commands.](img/ssh-command.PNG "SSH window")

Note: You can also use [Windows Terminal](https://docs.microsoft.com/windows/terminal/) or any favorite tool to connect - any SSH client will work.



### Task 2: Accessing the Ubuntu Data Science Virtual Machine - Graphical session


The Linux VM is already provisioned with X2Go Server and ready to accept client connections. To connect to the Linux VM graphical desktop, complete the following procedure on your client:

1. Download and install the X2Go client for your client platform from [X2Go](https://wiki.x2go.org/doku.php/doc:installation:x2goclient). You may have to give X2Go permission to bypass your firewall to finish connecting.

2. Make note of the virtual machine's public IP address, which you can find in the Azure portal by opening the virtual machine you created.

3. Run the X2Go client. If the "New Session" window doesn't pop up automatically, go to Session -> New Session.

4. On the resulting configuration window, enter the following configuration parameters:

    - **Session tab:**
        - **Host**: Enter the IP address of your VM, which you made note of earlier.
        - **Login:** Enter the username on the Linux VM.
        - **SSH Port:** Leave it at 22, the default value.
        - **Session Type:** Change the value to XFCE. Currently, the Linux VM supports only the XFCE desktop.

        ![X2GO](img/x2go.PNG "X2GO")

5. Select OK.

6. Click on the box in the right pane of the X2Go window to bring up the log-in screen for your VM.

7. Enter the password for your VM.

8. Select OK.

9. You should now see the graphical interface for your Ubuntu DSVM. Open RStudio from the Dekstop.

10. Execute a command in the RStudio console, for example **demo(lm.glm)**.


### Task 3: JupyterHub and JupyterLab


The Ubuntu DSVM runs JupyterHub, a multiuser Jupyter server. To connect, take the following steps:

1. Make note of the public IP address for your VM, by searching for and selecting your VM in the Azure portal.

2. From your local machine, open a web browser and navigate to https://your-vm-ip:8000, replacing "your-vm-ip" with the IP address you took note of earlier.

3. Your browser will probably prevent you from opening the page directly, telling you that there's a certificate error. The DSVM is providing security via a self-signed certificate. Most browsers will allow you to click through after this warning. Many browsers will continue to provide some kind of visual warning about the certificate throughout your Web session.

4. Enter the username and password that you used to create the VM, and sign in.

    ![Jupyter](img/jupyter.png "Jupyter")

5. Browse the many sample notebooks that are available.

JupyterLab, the next generation of Jupyter notebooks and JupyterHub, is also available. To access it, sign in to JupyterHub, and then browse to the URL https://your-vm-ip:8000/user/your-username/lab, replacing "your-username" with the username you chose when configuring the VM. 



### Task 4: Testing a bioinformatics workflow

1. Open the cloud shell and connect to your Data Science VM. Execute the SSH command (e.g. `ssh labuser@20.67.86.249` with your VM's IP address) to connect to the VM. Enter your VM password when prompted.

    ![An SSH shell prompt window displays commands.](img/ssh-command.PNG "SSH window")


2. Install necessary tools.

    ```bash
    sudo apt-get install -y bwa
    sudo apt install -y samtools
    sudo apt install -y bcftools
    ```

3. We created a storage account with a file share where the fastq files were added. Ask the your trainers for the credentials to mount the share on your VM.

4. Check that the share was mounted. You should be able to see the data and ref directories with the fastq files.

    ```bash
    cd /mnt/share
    ls
    ```

5. Create a directory and copy the data locally. The copy command will take a few minutes as 500MB are being copied.
    ```bash
    cd ~
    mkdir workshop
    cp -r /mnt/share/* workshop
    cd workshop
    ```

6. From the workshop directory, run the following commands individually to test one of classical bioinformatics workflows.
    ```bash
    bwa index ref/90-11-287_chr1_chr2_pJM1.fasta
    bwa mem ref/90-11-287_chr1_chr2_pJM1.fasta data/fastq/sub_R1.fa.gz data/fastq/sub_R2.fa.gz > data/aln/aln.sam 
    samtools view -@ 4 -h -S -b -o data/aln/aln.bam data/aln/aln.sam 
    samtools sort -T /tmp/aln.sorted -o data/aln/aln_sorted.bam data/aln/aln.bam 
    samtools index data/aln/aln_sorted.bam 
    bcftools mpileup -Ou -f ref/90-11-287_chr1_chr2_pJM1.fasta data/aln/aln_sorted.bam | bcftools call -mv -o output.vcf
    ```
