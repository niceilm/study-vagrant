# study-vagrant

> [VirtualBox](https://www.virtualbox.org/)나 [VMWare](http://www.vmware.com/kr)를 활용해서 CUI로 가상머신을 만들수 있는 프로그램

## Link
- https://www.vagrantup.com
- [Getting Started](https://www.vagrantup.com/docs/getting-started/)
- [Discover Vagrant Boxes](https://atlas.hashicorp.com/boxes/search)
- https://atlas.hashicorp.com

## Usecase
- 개발환경 단일화
- 서버 인프라 구성 공부

## Command
- ```vagrant init```
 - initializes a new Vagrant environment by creating a Vagrantfile
- ```vagrant up```
 - 머신을 시작하고 vagrant 환경을 적용한다.
- ```vagrant halt```
 - 머신을 중단 시킨다.
- ```vagrant destroy```
 - 머신을 중지시키고 관련 이미지를 모두 지운다.
- ```vagrant ssh```
 - 머신에 ssh 접속을 한다.
- ```vagrant ssh-config```
 - outputs OpenSSH valid configuration to connect to the machine
- ```vagrant connect```
 - connect to a remotely shared Vagrant environment
- ```vagrant global-status```
 - outputs status Vagrant environments for this user
- ```vagrant help```
 - vagrant 명령어 도움말을 본다.
- ```vagrant package```
 - 현재 실행중인 vagrant 환경을 box로 패키지한다.
- ```vagrant port```
 - 포트 매핑 상태를 보여준다.
- ```vagrant powershell```
 - connects to machine via powershell remoting
- ```vagrant provision```
 - 소프트웨어를 설치하거나 설정을 적용하는 것
- ```vagrant push```
 - deploys code in this environment to a configured destination
- ```vagrant rdp```
 - connects to machine via RDP
- ```vagrant reload```
 - 머신을 다시 시작하고 Vagrantfile을 새롭게 적용한다.
- ```vagrant login```
 - https://atlas.hashicorp.com 에 로그인 한다.
- ```vagrant share```
 - 현재 동작하는 머신을 웹상에서 공유할 수 있게 해준다.
 - http/https 를 지원
 - --name 특정 이름으로 subdomain 생성 가능
 - --domain 외부도메인으로 설정도 가능
- ```vagrant status```
 - 머신의 현재 상태를 보여줌
- ```vagrant suspend```
 - 머신을 잠시 중단 시킴 재시작은 ```vagrant up``` or ```vagrant resume```
- ```vagrant resume```
 - 서스펜드 상태의 머신을 깨움
- ```vagrant version```
 - vagrant의 현재 설치된 버전과 최신 버전을 보여줌
- ```vagrant snapshot```
 - 스냇샵 관리 명령어
- ```vagrant box```
 - box 관리 명령어
- ```vagrant plugin```
 - 플러그인을 관리

## PROVISIONING
> 가상 머신에다가 자동으로 소프트웨어를 설치하거나 설정을 적용하는 것
보통 ```vagrant up```을 할 경우 최초 한번만 수행

https://www.vagrantup.com/docs/provisioning/

##  SHARE
> 가상 머신 작업 내용을 퍼블릭하게 공개 할 수 있다.

https://www.vagrantup.com/docs/share/
https://www.vagrantup.com/docs/share/http.html

## Multi Instance
테스트를 하기 위해서 여러 인스턴스가 필요할 때가 있다.  
**이때 사용하기 위한 Vagrantfile**
```
Vagrant.configure("2") do |config|
  def define_instance(instance, port)
    instance.vm.network :"forwarded_port", guest: 22, host: port, id: 'ssh'
    instance.vm.box = "ubuntu/trusty64"
    instance.vm.provider "virtualbox" do |vb|
      vb.memory = "4096"
      vb.cpus = 2
    end
  end

  (0..5).each do |i|
    config.vm.define "instance#{i}" do |instance|
      define_instance(instance, 2222 + i)
    end
  end
end
```
총 6개의 인스턴스를 생성한다.
