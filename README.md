# ansible-basic

1. Ansible이란?
    - Python으로 구현된 오픈소스 IT 자동화 도구 -> 서버 설정, 미들웨어 설치, 소프트웨어 배포, 오케스트레이션 등 다양한 태스크 자동화 도구 (구성 관리 도구)
2. Ansible 이용 환경
    1. 실행 호스트 : Ansible 명령 실행 -> Ansible 설치 필요
    2. 대상 호스트 : 태스크 실행 -> SSH 접속 가능해야 함, Python 2.x 필요
    - 실행 호스트가 자기 자신에 대해 태스크 실행하는 경우, 로컬 연결로 접속 가능
3. Ansible 설치
    ```bash
    brew install ansible
    ```
4. Vagrant로 가상 서버 구축
    - 대상 호스트 준비 -> CentOS 7 가상 서버
    1. Vagrant, VirtualBox 설치
    2. Vagrantfile 작성
      ```ruby
      # -*- mode: ruby -*-
      # vi: set ft=ruby :

      Vagrant.configure("2") do |config|
        config.ssh.insert_key = false
        config.vm.box = "bento/centos-7"
        config.vm.network "private_network", ip: "192.168.34.21"
        config.vm.hostname = "soyoun-devops-ansible.dev"
      end
      ```
    3. Vagrant 실행
      - 실행 중이었다면 재시작 필요 
      
      1) Vagrant로 가상 서버 실행
      ```bash
      vagrant halt  #종료
      vagrant up 
      vagrant ssh
      ```
      
      2) ssh 명령으로 가상 서버 로그인
      ```bash
      ssh -i ./.vagrant.d/insecure_private_key vagrant@192.168.34.21
      ```
5. ansible.cfg
    - Ansible 설정 INI 파일 -> 참조 파일 경로 변경, 환경변수 설정, 명령의 동작 변경 등
6. 인벤토리 
    - ansible, ansible-playbook 명령으로 태스크를 실행할 대상 호스트 정보 기술 파일 -> Ansible에서는 인벤토리에 기술된 호스트만 처리 (localhost는 디폴트로 포함)
    - INI, YAML 등 여러 형식으로 작성 가능
    
    - hosts 파일 
    1) 별명으로 호스트 지정, 그룹화
    ```bash
    [development]
    vm1 ansible_ssh_host=192.168.34.21 ansible_ssh_user=vagrant ansible_ssh_private_key_file=~/.vagrant.d/insecure_private_key
    ```
    
    2) 그룹에 변수 지정
    ```bash
    [development]
    soyoun-devops-ansible.dev
    
    [development:vars]
    ansible_ssh_user=vagrant 
    ansible_ssh_private_key_file=~/.vagrant.d/insecure_private_key
    ```
    
7. ansible 명령
    - 인벤토리의 호스트에 대해 모듈을 실행하는 명령 -> 인벤토리, 대상 호스트의 패턴, 실행할 모듈 지정 필요
    - 서버에 SSH 로그인해서 명령을 실행하는 형태의 작업들을 자동화할 수 있게 해줌
    1) 대상 호스트 패턴
        : 인벤토리에서 어떤 호스트에 대해 처리할지 지정
        
        ```bash
        # --list-hosts : 대상 호스트명 출력, 모듈 실행 안됨
        ansible all -i hosts --list-hosts
        
        ansible development -i hosts --list-hosts
        
        ansible soyoun-devops-ansible.dev -i hosts2 --list-hosts
        ```
        
    2) 모듈
        : 대상 호스트에서 실행할 처리를 구현한 라이브러리 -> 파일 전송, 파일 조작, 패키지 설치, 서비스 실행, git, 클라우드 서비스 제어 등 기능 제공 (-m 옵션)
        
    3) 실행
        ```bash
        ansible vm1 -i hosts -m shell -a uname
        
        ansible development -i hosts -m shell -a 'uname -a'
        
        ansible vm1 -i hosts -m shell -a uname -v
        
        ansible vm1 -i hosts -m shell -a uname -vvv
        
        
        
        #에러!!!
        ansible soyoun-devops-ansible.dev -i hosts2 -m shell -a uname         
        
        #해결방법
        1) /etc/hosts 파일에 hostname 설정
        2) soyoun-devops-ansible.dev ansible_ssh_host=192.168.34.21
        
        ```
        
        
   
1. Playbook 이란?
    : Ansible에서 대상 호스트에 대한 태스크를 기술하는 YAML 파일
    
    playbook-sample.yml
    ```bash
    ---
    - hosts: vm1
      become: true
      tasks:
      - name: Apache 설치
        yum: name=httpd state=latest

      - name: Apache 실행
        service: name=httpd state=started enabled=yes
    ```
    
    ```bash
    
    #에러
    ansible-playbook -i hosts playbook-sample.yml
    ```
    
    
https://docs.ansible.com/ansible/2.4/intro_windows.html#id5
