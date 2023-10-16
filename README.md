## Elasticsearch training lab
이 프로젝트는 Elasticsearch 8.1.3을 교육하고 실험하기 위한 간단한 랩 환경입니다.

CentOS에서 실행되는 노드를 구축하고 로컬이 아닌 IP에서 Elasticsearch를 실행하는 데 필요한 시스템 설정으로 노드를 프로비저닝합니다. 이는 소박한 프로비저닝 방법이지만 우리의 요구 사항에 적합합니다.

랩은 순전히 교육 도구로 설계되었습니다. 프로덕션 클러스터에 대한 모든 모범 사례를 사용하지 않으며 프로덕션 클러스터에 적합한 것으로 간주되어서는 안 됩니다.

Elastic Certified Engineer를 공부 중이거나 Elasticsearch Engineer I 및/또는 II 과정을 수강한 경우, 이 실습은 시험에 사용되는 환경에 익숙해지고 과정에서 실습을 실행하는 데 도움이 될 것입니다. 가장 큰 차이점은 여기서는 Vagrant를 사용하므로 ssh 대신 vagrant ssh를 실행해야 한다는 것입니다. SSH를 직접 사용하려면 공개 키를 VM에 복사하기 위한 몇 가지 작업을 수행해야 합니다.

### Dependencies
These two archives are required:
- [Elasticsearch 8.1.3](https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.1.3-linux-x86_64.tar.gz)
- [Kibana 8.1.3](https://artifacts.elastic.co/downloads/kibana/kibana-8.1.3-linux-x86_64.tar.gz)

### Settings summary
- Each VM is given 4G RAM.
- Elasticsearch is configured (in `jvm.options`) with a 512M heap - [50% available RAM](https://www.elastic.co/guide/en/elasticsearch/reference/current/heap-size.html).
- The number of file descriptors [is increased](https://www.elastic.co/guide/en/elasticsearch/reference/current/file-descriptors.html).
- The number of threads [is increased](https://www.elastic.co/guide/en/elasticsearch/reference/current/max-number-of-threads.html).
- The `mmap` count limit [is increased](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html).
- [Disable swapping](https://www.elastic.co/guide/en/elasticsearch/reference/current/setup-configuration-memory.html#swappiness)

### Running
- Install VirtualBox
- Install Vagrant
- Clone this repository
- Place the Elasticsearch and Kibana archives into the repository root
- `cd` into the repo
- Run `vagrant up`

`Vagrantfile`을 수정하지 않고 Vagrant는 두 개의 VM( Elasticsearch 노드 1개(`10.0.200.101`)와 Kibana 인스턴스 1개(`10.0.200.104`) )을 가동합니다.

Elasticsearch를 시작하려면 `vagrant ssh node1`을 실행하고 `elasticsearch-8.1.3/bin/elasticsearch`를 실행하면 됩니다.

Kibana를 시작하려면 `vagrant ssh node4`를 실행하고 `kibana-8.1.3/bin/kibana`를 실행해야 합니다.

더 많은 노드가 필요한 경우 `node2` 및 `node3`에 대한 프로비저너의 주석 처리를 해제한 다음 `vagrant up`을 다시 실행할 수 있습니다. 해당 노드에서 Elasticsearch를 시작하면 자동으로 클러스터에 참여하고 샤드 재할당이 시작됩니다.

### Resetting
환경을 사용해왔고 모든 것을 불태우고 처음부터 시작하고 싶거나 필요하다면 `vagrant destroy`를 사용할 수 있습니다. 이렇게 하면 VM이 삭제되므로 다음에 `vagrant up`할 때 완전히 새로운 클러스터를 얻게 됩니다.
