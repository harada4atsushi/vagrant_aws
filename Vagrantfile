Dotenv.load
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "dummy_for_ec2" # dummyのbox
  config.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box" # dummyのbox
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider :aws do |aws, override|
    aws.access_key_id     = ENV['ACCESS_KEY_ID']
    aws.secret_access_key = ENV['SECRET_ACCESS_KEY']
    aws.keypair_name = ENV['KEY_PAIR_NAME']

    aws.region = "ap-northeast-1" # Tokyoリージョン
    aws.ami = "ami-c9562fc8" # Amazon Linux PV EBS-Backed 64bit
    aws.instance_type = "t1.micro" # microインスタンス
    aws.security_groups = ["https", "ssh"] # 割り当てるセキュリティグループ
    aws.elastic_ip = true # 自動的にElastic IPを取得して割り当てる

    override.ssh.username = "ec2-user" # Amazon Linuxの場合はec2-user
    override.ssh.private_key_path = "~/.ssh/id_rsa"
  end
end
