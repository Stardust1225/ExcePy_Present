# Page for Benchmark **ExcePy**
This page is for Python benchmark **ExcePy**.
All the experiments are under Python 3.7, Ubuntu 18.
For some specific test cases, you need to run under Python 3.6 or Python 2.7.

##### View the bug patterns and fix patterns.
##### Download the Exception Data
##### Download the Execution Path

### 1 Download
You can download the benchmark here, and unzip the file via the following commands. Also, the results can be referenced to check whether the files are complete.
```
> unzip ExcInPy.zip
> ls
data  ExcInPy.py  record.json
```

For each project in the benchmark, we use a fold with the same name to store the test cases and scripts.

### 2 Run the test cases.
We provide two ways to run the test cases and check the traceback.

#### 2.1 Run from the scripts directly
You can run the test cases in one fold via executing the corresponding scripts directly. For example, if you want to run a test case in project sympy, you can refer to the following commands.
```
> cd data/sympy/

> chmod +x init.sh & dos2unix init.sh
dos2unix: converting file init.sh to Unix format

>sudo ./init.sh
Cloning into 'sympy'...
...
Resolving deltas: 100% (258589/258589), done.

> chmod +x test1.sh & dos2unix test1.sh
dos2unix: converting file test1.sh to Unix format...

> pip3 show sympy

> ./test1.sh
HEAD is now at 0fc1bf2ea4 stats: Made changes to return value in entropy()
HEAD is now at 425353e982 Merge pull request #20543 from oscarbenjamin/pr_master_whitelist
Obtaining file:///./Documents/data/sympy/sympy
Collecting mpmath>=0.19 (from sympy==1.8.dev0)
  Downloading https://files.pythonhosted.org/packages/d4/cf/3965bddbb4f1a61c49aacae0e78fd1fe36b5dc36c797b31f30cf07dcbbb7/mpmath-1.2.1-py3-none-any.whl (532kB)
    100% |████████████████████████████████| 542kB 1.3MB/s 
Installing collected packages: mpmath, sympy
  Running setup.py develop for sympy
Successfully installed mpmath-1.2.1 sympy
Traceback (most recent call last):
  File "test1.py", line 6, in <module>
    H(B)
  File "./sympy/sympy/sympy/stats/rv_interface.py", line 137, in entropy
    return sum([-prob*log(prob, base) for prob in pdf.dict.values()])
AttributeError: 'Density' object has no attribute 'values'
```
Before running test cases in one project, you need to run the init.sh firstly to download the source code and install the dependencies. Then you can execute the corresponding scripts to change the source code to the buggy version, build the project from the source code, and execute the test case.

After executing the scripts, if you want to re-run the test case, you just need to execute the Python file. For example, if you want to re-run test case 1 in project sympy, you only need to use the following command:
```
> python3 test1.py
Traceback (most recent call last):
  File "test1.py", line 6, in <module>
    H(B)
  File "./sympy/sympy/sympy/stats/rv_interface.py", line 137, in entropy
    return sum([-prob*log(prob, base) for prob in pdf.dict.values()])
AttributeError: 'Density' object has no attribute 'values'
```
The traceback is both in the record.json and in the scripts, you can compare the running results with the traceback.

**notice ! !**
Before building from the source code, you need to uninstall the project. Otherwise, the test case would use the APIs from the pre-installed code other than the buggy code.

#### 2.2 Run from the APIs in the Python file.
We provide a Python file for users to call: ExcInPy.py. You can use this file to complete all the processes and get running results. We provide the following APIs:

##### 2.2.1 Get help
You can run the following command to get help:
```
> python3 ExcInPy.py --help
usage: ExcInPy.py [-h] [--list] [--init_project PROJECT]
                         [--init_test HASH_ID_INIT_TEST]
                         [--run_test HASH_ID_RUN]
                         [--get_traceback HASH_ID_TRACEBACK]

optional arguments:
  -h, --help            show this help message and exit
  --list                List exceptions
  --init_project PROJECT
                        Init corresponding project
  --init_test HASH_ID_INIT_TEST
                        Use exception ID to get corresponding fault version
                        and test code
  --run_test HASH_ID_RUN
                        Run corresponding test code
  --get_traceback HASH_ID_TRACEBACK
                        Get the corresponding traceback of the exception

```
##### 2.2.2 Get project information
To know the basic information of test cases, you can run the following command:
```
> python3 ExcInPy.py --list
project:  cpython
ID: 1      TYPE: AttributeError      COMMIT: https://github.com/python/cpython/commit/a4c377cde9b010f6bee5e5e0a15e8545228d31e2
ID: 2      TYPE: ZeroDivisionError      COMMIT: https://github.com/python/cpython/commit/4762382d6346abdef810cdf0a6101cbc1ec5952e
ID: 3      TYPE: AttributeError      COMMIT: https://github.com/python/cpython/commit/f70ce68c9cce352a34cd30ecf5168d93bc379817
ID: 4      TYPE: AttributeError      COMMIT: https://github.com/python/cpython/commit/8eba090694d661d98eb24e6894b0c3e2c5c76ba2
...
```

To know test cases in one project, for example, in project sympy, you can run the following command:
```
> sudo python3 ExcInPy.py --init_project sympy
dos2unix: converting file ./data/sympy/test52.sh to Unix format...
dos2unix: converting file ./data/sympy/test17.sh to Unix format...
dos2unix: converting file ./data/sympy/test25.sh to Unix format...
dos2unix: converting file ./data/sympy/test53.sh to Unix format...
dos2unix: converting file ./data/sympy/test40.sh to Unix format...
dos2unix: converting file ./data/sympy/test10.sh to Unix format...
dos2unix: converting file ./data/sympy/test27.sh to Unix format...
...
Cloning into 'sympy'...
...
Current project has tests: 
89
90
91
92
93
...
148
```
This command would download the source code of the project, and print the ID of test cases in this project.

##### 2.2.3 Get test cases
You can specify the ID of test cases to obtain the corresponding script and test code.
For example, if you want to get test 89 in project sympy.
```
> python3 ExcInPy.py --init_test 89
HEAD is now at 0fc1bf2ea4 stats: Made changes to return value in entropy()
HEAD is now at 425353e982 Merge pull request #20543 from oscarbenjamin/pr_master_whitelist
...
  Running setup.py develop for sympy
Successfully installed mpmath-1.2.1 sympy
Traceback (most recent call last):
  File "test1.py", line 6, in <module>
    H(B)
  File "/home/xin/Documents/sympy/sympy/stats/rv_interface.py", line 137, in entropy
    return sum([-prob*log(prob, base) for prob in pdf.dict.values()])
AttributeError: 'Density' object has no attribute 'values'

> ls
data  ExcInPy.py  init.sh  record.json  sympy  test1.py  test1.sh
```
This command would finish the processes of building from source code and executing test cases. 

##### 2.2.4 Run the test again
After the first run of the test case, you can run the test case again without re-install the project.
For example, after running the command,
```
python3 ExcInPy.py --init_test 89
```
you can re-run this test case via 
```
> python3 ExcInPy.py --run_test 89
Traceback (most recent call last):
  File "test1.py", line 6, in <module>
    H(B)
  File "/home/xin/Documents/sympy/sympy/stats/rv_interface.py", line 137, in entropy
    return sum([-prob*log(prob, base) for prob in pdf.dict.values()])
AttributeError: 'Density' object has no attribute 'values'
```
This is functionally equal to the command
```
python3 test1.py
```

##### 2.2.5 Get traceback of a test case
You can get the traceback for one test case via specifying the test ID.
For example, if you want to get the traceback for test 89.
```
> python3 ExcInPy.py --get_traceback 89
Traceback: 
  File "test1.py", line 5, in <module>
    H(B)
  File "./sympy/sympy/stats/rv_interface.py", line 137, in entropy
    return sum([-prob*log(prob, base) for prob in pdf.dict.values()])
AttributeError: 'Density' object has no attribute 'values'
```

##### 2.2.6 Get Install commands of a project
If you change the source code and want to re-install the project,
you can get the install commands also via specifying the test ID.
For example, if you want to re-install the project sympy for test 89.
```
> python3 ExcInPy.py --get_install_commands 89
Install commands: 
pip3 install -e .
```
You can run this command under the source code directory to re-install the project.