Dotenv.load
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "dummy_for_ec2" # dummyのbox
  config.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box" # dummyのbox
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.omnibus.chef_version = :latest

  config.vm.provider :aws do |aws, override|
    aws.access_key_id     = ENV['ACCESS_KEY_ID']
    aws.secret_access_key = ENV['SECRET_ACCESS_KEY']
    aws.keypair_name = ENV['KEY_PAIR_NAME']

    aws.region = "ap-northeast-1" # Tokyoリージョン
    aws.ami = "ami-c9562fc8" # Amazon Linux PV EBS-Backed 64bit
    aws.instance_type = "t1.micro" # microインスタンス
    aws.security_groups = ["https", "ssh"] # 割り当てるセキュリティグループ
    aws.elastic_ip = true # 自動的にElastic IPを取得して割り当てる
    aws.tags = { 
      'Name' => 'redmine'
    }

    # tty以外のsudoを許可
    aws.user_data = <<-EOH
      #!/bin/sh
      sed -i -e 's/^\\(Defaults.*requiretty\\)/#\\1/' /etc/sudoers
    EOH

    override.ssh.username = "ec2-user" # Amazon Linuxの場合はec2-user
    override.ssh.private_key_path = "~/.ssh/id_rsa"
  end

  config.vm.provision :chef_solo do |chef|
    chef.log_level = :debug
    chef.cookbooks_path = ["./berks-cookbooks", "./site-cookbooks" ]

    chef.json = {
      #chef_environment: "production",
      rbenv: {
        rubies: ["2.1.1"],
        global: "2.1.1"
      },
      mysql: {
        server_root_password: ENV["MYSQL_PASSWORD"],
      },
    }
    chef.run_list = [
      "ruby_build",
      "rbenv::system",
      "mysql::client",
      "mysql::server",
      "passenger_apache2",
      "vim"
    ]
  end

  #config.vm.network :forwarded_port, host: 10080, guest: 80
  config.ssh.forward_agent = true # SSH先でもローカルのssh-agentに登録されている鍵が使用出来る
end
