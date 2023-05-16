# amazon-cloudwatch-starter

## Amazon CloudWatch Logs
### 로그 그룹
> CloudWatch 에서 좌측 네비게이션 바에서 로그 > 로그 그룹 으로 이동한다.  
> 테이블 상단의 버튼인 '로그 그룹 생성' 버튼을 클릭하여 로그 그룹을 생성할 수 있다.

### 기능
> **로그 데이터 쿼리**   
> CloudWatch Logs Insights를 사용하여 로그 데이터를 대화식으로 검색하고 분석할 수 있습니다. 
> 운영상의 문제에 보다 효율적이고 효과적으로 대처할 수 있도록 쿼리를 수행할 수 있습니다. 
> `CloudWatch Logs Insights`에는 몇 가지 간단하지만 강력한 명령과 함께 특별히 구축된 쿼리 언어가 포함되어 있습니다.  
> 
> **Amazon EC2 인스턴스에서 로그 모니터링**   
> CloudWatch Logs를 사용하면 로그 데이터를 통해 애플리케이션과 시스템을 모니터링할 수 있습니다. 
> 예를 들어 CloudWatch Logs에서는 애플리케이션 로그에서 발생하는 오류의 수를 추적하고 오류 비율이 지정한 임계값을 초과할 때마다 알림을 전송할 수 있습니다.  
> CloudWatch Logs는 모니터링하는 데 로그 데이터를 사용하므로 코드를 변경할 필요가 없습니다. 
> 예를 들어 특정 리터럴 문자(예: "NullReferenceException")에 대한 애플리케이션 로그를 모니터링하거나 
> 로그 데이터의 특정 위치(예: Apache 액세스 로그의 "404" 상태 코드)에서 리터럴 문자의 출현 횟수를 계산할 수 있습니다. 
> 검색할 단어가 발견되면 CloudWatch Logs는 지정한 CloudWatch 지표로 데이터를 보고합니다.
> 
> **로그 보존**   
> 기본적으로 로그는 무기한으로 보존되며 만료되지 않습니다. 로그 그룹별로 보존 정책을 조정할 수 있고 무기한 보존 유지 또는 10년 및 하루 중 보존 기간을 선택합니다.

### 역할(role) 추가
> 기존 EC2 인스턴스에 연결된 역할(role)에 다음 정책 추가: `CloudWatchAgentServerPolicy`

### 통합 CloudWatch Agent 
> **에이전트 설치 및 설정**    
> ```shell
> # 에이전트 설치  
> sudo yum install -y amazon-cloudwatch-agent
> 
> # 에이전트 설정
> cd 
> vi config.json
> {
>   ...
> 	"logs": {
> 		"logs_collected": {
> 			"files": {
> 				"collect_list": [
> 					{
> 						"file_path": "<로그 파일 경로>",
> 						"log_group_name": "<로그 그룹 이름>"
> 					}
> 				]
> 			}
> 		}
> 	}
>   ...
> }
> :wq
> 
> # 에이전트 실행
> sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
> -a fetch-config \
> -m ec2 \
> -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json -s
> 
> # 실행 확인
> sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status
> ```

### 지표 필터 생성
> 로그에 설정한 기간 동안 특정 문자가 몇 번 이상 나오면 알람을 받도록 설정할 수 있다.  
> 예를 들어 10분 동안 ERROR 메시지가 100개가 넘어가면 이메일로 알람을 받도록 설정할 수 있다.  
> 
> **설정**  
> CloudWatch 에서 로그 > 로그 그룹 > 지표 필터를 생성할 로그 그룹을 클릭하여 이동한다.      
> 로그 그룹 내에서 작업 > 지표 필터 생성 클릭한다.  
> 
> **확인**  
> CloudWatch 에서 지표 > 모든 지표 > 찾아보기 에서 위에서 설정한 지표 필터의 이름을 찾아서 클릭하여 확인한다.  
> 
> **경보 생성**  
> CloudWatch 에서 경보 > 모든 경보 > 경보 생성을 클릭한다.
> 
> **참조사이트**  
> [Amazon CloudWatch 총 정리합니다 - 3: Demo (지표,로그,알람)](https://www.youtube.com/watch?v=8t1kmIDtfqc)  

### 참조사이트
> [2. EC2 Linux Command Log 수집 - CW Agent](https://aws-diary.tistory.com/74)  

---

## CloudWatch Metrics
### 데이터 저장
> CloudWatch 에서 Metrics, 지표는 최대 15개월의 데이터를 저장한다.  

### 데이터 포인트
> 지표를 구성하는 시간-값 데이터 단위이다.  
> 시간은 초 단위까지 이며 yyyy-MM-dd HH:mm:ssZ 형식이다.   
> 예시: 2023-04-16T23:59:59Z  
> 타임존은 UTC 로 하는 것을 매우 권장한다. 내부적인 통계 및 알람 등에서 UTC 를 기준으로 하기 때문이다.  
> 
> **데이터 포인트의 Resolution**  
> 데이터가 얼마나 자주 수집되는지를 나타내는 개념.  
> 기본적으로 60초 단위로 수집하며, High resolution 모드에서는 1초 단위로 수집한다.  
> 일반적으로는 60의 배수로 설정이 가능하며, 60초 미만의 단위로 사용하려면 High resolution 모드로 사용해야 하며 1, 5, 10, 30 으로 설정할 수 있다.   
> 
> **보관 기간**  
> 60초 미만은 최대 3시간만 보관한다.  
> 60초(1분)는 15일을 보관한다.   
> 300초(5분)는 63일을 보관한다.    
> 1시간(3600초)는 455일(15개월)을 보관한다.  
> 작은 단위의 보관 기간은 큰 단위로 계속 합쳐진다. 예를 들어 1분 단위는 15일 동안만 확인이 가능하며, 15일이 지나면 5분 단위로만 확인이 가능하다.
> 그 이후 63일이 지나면 1시간 단위로만 확인이 가능하다.  
> 
> **주의 사항**  
> 2주 이상 데이터가 업데이트 되지 않은 Metric 의 경우 콘솔에서 보이지 않음.  
> 모든 콘솔에서 사라지며 aws cli 로만 확인이 가능해진다.

### 통합 CloudWatch Agent
> **에이전트 설치 및 설정**  
> ```shell
> # 에이전트 설치  
> sudo yum install -y amazon-cloudwatch-agent
> 
> # 에이전트 설정
> cd 
> vi config.json
> {
>   ...
>   "metrics": {
>       ...
>       "metrics_collected": {
>           "cpu": {
>               "measurement": [
>                   "cpu_usage_idle",
>                   "cpu_usage_iowait",
>                   "cpu_usage_user",
>                   "cpu_usage_system"
>               ],
>               "metrics_collection_interval": 30,
>               "resources": [
>                   "*"
>               ],
>               "totalcpu": false
>          },
>          "disk": {
>               "measurement": [
>                   "used_percent",
>                   "inodes_free"
>               ],
>               "metrics_collection_interval": 30,
>               "resources": [
>                   "*"
>               ]
>          },
>          "diskio": {
>               "measurement": [
>                   "io_time",
>                   "write_bytes",
>                   "read_bytes",
>                   "writes",
>                   "reads"
>               ],
>               "metrics_collection_interval": 30,
>               "resources": [
>                   "*"
>               ]
>          },
>          "mem": {
>              "measurement": [
>                  "mem_used_percent"
>              ],
>              "metrics_collection_interval": 30
>         },
>         "netstat": {
>             "measurement": [
>                 "tcp_established",
>                 "tcp_time_wait"
>             ],
>             "metrics_collection_interval": 30
>         },
>         "swap": {
>             "measurement": [ 
>                 "swap_used_percent"
>             ],
>             "metrics_collection_interval": 30
>         }
>   }
>   ...
> }
> :wq
> 
> # 에이전트 실행
> sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
> -a fetch-config \
> -m ec2 \
> -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json -s
> 
> # 실행 확인
> sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status
> ```
