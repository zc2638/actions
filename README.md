# Actions
Github Actions

### Limit
1. 每个repository可以同时执行20个workflow
2. 一小时内在repository中的所有actions中执行多达1000个API请求
3. workflow中的每个job都可以运行长达6个小时的执行时间
4. 每个repository的所有workflow中最多可以同时运行20个job


### Syntax

#### name	
（可选）工作流程的名称。GitHub在repository的操作页面上显示workflow的名称。如果省略此字段，GitHub将设置name为workflow的文件名

#### on 		
（必须）触发workflow的GitHub事件的名称，可以提供单个事件string，array事件或事件配置map，以调度工作流或将工作流的执行限制为特定文件，标记或分支更改。有关可用事件的列表，请参阅“ [**触发工作流程的事件**](https://help.github.com/en/articles/events-that-trigger-workflows)”

#### job
```
jobs:
  my_first_job:				# 自定义的job变量名
    name: My first job		# 显示的job名称
    timeout-minutes: 360 	# job超时时间，默认360分钟
    runs_on: ubuntu-latest	# 环境，支持：ubuntu-latest, ubuntu-18.04, ubuntu-16.04, windows-latest, windows-2019, windows-2016, macOS-latest, macOS-10.14
    steps:
    - name: test step 		# 显示的step名称
    
      # step的唯一标识，可在if表达式中引用
      id: test_step			
      
      # 满足条件执行
      if: github.event_name == 'push' || github.event_name == 'pull_request'
      
      # 设置环境变量
      env:
        HELLO: Hello World!
      
      # 指定执行命令的方式(bash、sh、cmd、pwsh、python等)
      shell: bash
      
      # 不指定shell，使用系统默认shell命令
      run: |
        echo $HELLO
        echo $HOME
      
      # 设为true，允许当此step失败时job通过
      continue-on-error: true
      
      # step超时时间
      timeout-minutes: 60
        
  my_second_job:
    name: My second job
    needs: my_first_job		# 同步，必须my_first_job成功才会执行，否则直接失败
  my_third_job:
    name: My third job
    needs: [my_first_job, my_second_job]    
```
```
steps:
  - uses: actions/setup-node@master  			# 选择一个分支
    
    # 作为使用的action的环境变量INPUT_FIRST_NAME和INPUT_LAST_NAME传入
    with:
      first_name: Hello
      last_name: World
      
  - uses: actions/setup-node@74bc508 			# 选择一次提交
  - uses: actions/setup-node@v1      			# 选择一个主要版本   
  - uses: actions/setup-node@v1.2    			# 选择一个次要版本 
  - uses: ./.github/actions/my-action 			# 选择本仓库下的action
  - uses: docker://alpine:3.8 					# 选择docker hub上的镜像
  - uses: docker://gcr.io/cloud-builders/gradle # 选择docker公有仓的镜像
```
[系统环境变量](https://help.github.com/en/articles/virtual-environments-for-github-actions)  
[Contexts和表达式语法](https://help.github.com/en/articles/contexts-and-expression-syntax-for-github-actions)


