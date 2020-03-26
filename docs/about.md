# About

>for wiki
power by gitbook & plug/theme by zitiao

- plug/theme:[https://github.com/zitiaocn/simple](https://github.com/zitiaocn/simple)
- document:[http://zitiao.org/deploy/](http://zitiao.org/deploy/)

## background process
```
setsid gitbook serve .
```

## kill port
```
netstat -anp | grep 4000
kill -9 XXX
```

### Step 1

Run command-line as an Administrator. Then run the below mention command. type your port number in **yourPortNumber**

> netstat -ano | findstr :**yourPortNumber**

```
C:\Users\yaokunyi>netstat -ano | findstr :35729
TCP    0.0.0.0:35729          0.0.0.0:0              LISTENING       5396
TCP    [::]:35729             [::]:0                 LISTENING       5396 
```

See the PID ? In here the port is 5396 (process identifier)

### Step 2

Then you execute this command after identify the PID.
(/F forcefully terminates the process)

> taskkill /PID **typeyourPIDhere** /F

```
C:\Users\yaokunyi>taskkill /PID 5396 /F
SUCCESS: The process with PID 5396 has been terminated.
```

P.S. Run the first command again to check if process is still available or not. You'll get empty line if process is successfully ended

***
By [stackoverflow  how to kill port](https://stackoverflow.com/questions/39632667/how-to-kill-a-currently-using-port-on-localhost-in-windows#)

