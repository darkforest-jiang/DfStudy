sql
[toc]

# 创建表
create table department(
	department_id int not null auto_increment comment '部门Id 主键自增',
	company_id int not null default 0 comment '所属机构Id',
	department_name varchar(50) character set utf8mb4 collate utf8mb4_0900_ai_ci not null comment '部门名称',
	create_time datetime not null default CURRENT_TIMESTAMP comment '创建时间',
	sync_time datetime null comment '同步时间',
	primary key (department_id) using btree
) engine=InnoDB default charset=utf8mb4 collate=utf8mb4_0900_ai_ci row_format=dynamic comment='部门表';

# 修改字段
alter table department modify column company_id int null comment '机构id';


