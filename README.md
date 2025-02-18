# jenkins
How to install jenkins

# Installing openJDK

- First go here and download the latest LTS Release.: https://adoptium.net/temurin/

![openjdk download page](https://i.imgur.com/zWn1uGc.png)

- After download, open the installer and do the following steps:

    ![1](https://i.imgur.com/0rpkQvT.png)

    ![1](https://i.imgur.com/i2TNXRg.png)
  
   ![2](https://i.imgur.com/H4mCTnD.png)

  ![3](https://i.imgur.com/OUICZ8g.png)

    ![1](https://i.imgur.com/VmmS6Iz.png)

  # Editing System Enviornments

-  Open run dialog using Windows button  + R and type 
```sysdm.cpl ,3```

- Edit enviornment variables

  ![1](https://i.imgur.com/TEVhepI.png)

![HERE BRO HERE](https://i.imgur.com/u239to5.png)

![SystemPropertiesAdvanced_jPJnGrPV68.png](https://i.imgur.com/va3bjTa.png)

![SystemPropertiesAdvanced_EWY6bb1OGw.png](https://i.imgur.com/1f6icQJ.png)


# Installing Jenkins

![1](https://i.imgur.com/luMvGuK.png)

![2](https://i.imgur.com/NoKJKPx.png)

![3](https://i.imgur.com/MeI4MUz.png)

![4](https://i.imgur.com/94IJQWC.png)

![5](https://i.imgur.com/pqDo5pa.png)

![6](https://i.imgur.com/UlVeVQ8.png)

![7](https://i.imgur.com/vftPAAJ.png)


# Using Jenkins

- Visit [http://localhost:8080/](http://localhost:8080/)

![1](https://i.imgur.com/Xd4xKMm.png)

- Open the target file with notepad and find the password, you can alternatively do this:

  - ![Windows button + R](https://i.sstatic.net/ktZiD.jpg)
 
  - Type this and press enter:
 
  ```
  cmd /c "type C:\ProgramData\Jenkins\.jenkins\secrets\initialAdminPassword | clip"
  ```
  
  - The password will be copied to your clipboard


![2](https://i.imgur.com/Kl7PrcY.png)



