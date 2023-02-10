## Terraform问题总结

- elb创建时，subnet要调用huawei_vpc_subnet.xxx.subnet_id，用huawei_vpc_subnet.xxx.id会报错找不到subnet资源

  ```
   Error: Error: listener d9415786-5f1a-428b-b35f-2f1523e146d2 not found: Resource not found: [GET https://elb.ap-southeast-2.myhuaweicloud.com/v2/05df630d2c800f7b2ffdc007e96613df/elb/listeners/d9415786-5f1a-428b-b35f-2f1523e146d2], error message: {"error":{"message":"listener d9415786-5f1a-428b-b35f-2f1523e146d2 could not be found","code":"ELB.9800"}}
  │ 
  │   with module.elb.huaweicloud_lb_pool.pool_1,
  │   on ../elb/main.tf line 44, in resource "huaweicloud_lb_pool" "pool_1":
  │   44: resource "huaweicloud_lb_pool" "pool_1" {
  ```

- 报错包含“neutron”，一般都是region写错了

- 新建专业vpn怎么关联对端网关

  ```
  resource "huaweicloud_vpn_gateway" "hk" {
    name               = var.hk_name
    vpc_id             = var.hk_vpc_id
    local_subnets      = ["192.168.1.0/24"]
    connect_subnet     = "192.168.3.0/24"
    availability_zones = ["ap-southeast-1b", "ap-southeast-1c"]
  
    master_eip {
      bandwidth_name = "hk1"
      type           = "5_bgp"
      bandwidth_size = 5
      charge_mode    = "traffic"
    }
  
    slave_eip {
      bandwidth_name = "hk2"
      type           = "5_bgp"
      bandwidth_size = 5
      charge_mode    = "traffic"
    }
  }
  
  resource "huaweicloud_vpn_customer_gateway" "hk" {
    name   = "bkk_ip"
    ip     = huaweicloud_vpn_gateway.hk.master_eip[0].ip_address
    region = "ap-southeast-1"
  }
  ```

  
