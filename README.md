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
      config.vm.box = "bento/centos-7.1"
      config.vm.network "private_network", ip: "192.168.34.21"
      config.vm.hostname = "soyoun-devops-ansible.dev"
    end
    ```
    3. Vagrant 실행
        - 실행 중이었다면 재시작 필요 
        ```bash
        vagrant halt  #종료
        vagrant up 
        vagrant ssh
        ```
5. ansible.cfg
    - Ansible 설정 INI 파일 -> 참조 파일 경로 변경, 환경변수 설정, 명령의 동작 변경 등
6. 
      
      
      
