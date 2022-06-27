# enildo..

drop database dbdistribuidora;

create database dbdistribuidora;
use dbdistribuidora;

create table tbproduto(
	codigobarras char(14) primary key unique,
	nomeprod varchar(200) not null,
    precounitarioprod decimal(7,2) not null,
    qtd int not null
);

select * from tbproduto;

create table tbfornecedor(
	codigo char(10) primary key,
	nomefornecedor varchar(200) not null,
    cnpj varchar(14) not null unique,
    telfornecedor char(11)
);

create table tbcliente(
	idcli int auto_increment primary key,
    nomecli varchar(200) not null,
    numend char(6) not null,
    cep char(8) not null,
    compend varchar(50) not null
);

create table tbbairro(
	idbairro int primary key auto_increment,
    bairro varchar(200) not null
);

create table tbcidade(
	idcidade int primary key auto_increment,
    cidade varchar(200) not null
);

create table tbestado(
	iduf int primary key auto_increment,
    uf varchar(200) not null
);

create table tbclijuridico(
	idcli int,
	cnpj char(18) not null primary key,
    ie bigint 
);

create table tbclifisico(
	idcli int,
	cpf char(11) not null primary key,
    rg char(9) not null,
	nasc date,
    rgdigito char(1) not null
);

create table tbvenda(
	numerovenda int not null primary key,
	datavenda date,
    totalvenda decimal(7,2) not null,
    idcli int,
    nf int
);

create table tbcompra(
	notafiscal int primary key,
	datacompra date not null,
    valortotal decimal(7,2),
    qtdtotal int not null,
    codigo char(10)
);

create table tbitemvenda(
	numerovenda int not null primary key auto_increment,
    codigobarras char(14),
	valoritem decimal(7,2) not null,
    qtd int not null
);

create table tbitemcompra(
	notafiscal int primary key,
    codigobarras char(14),
    valoritem decimal(7,2) not null,
    qtd int not null
);

create table tbnotaFiscal(
	nf int primary key,
    totalnota decimal(7,2) not null,
	dataemissao datetime not null
);

create table tbendereco(
	Logradouro varchar(200) not null,
    cep char(8) primary key,
    idbairro int not null, /* FK */
    idcidade int not null, /* FK */
    iduf int not null /* FK */
);

alter table tbclijuridico add constraint fk_idcli__ foreign key (idcli) references tbcliente(idcli);
alter table tbclifisico add constraint fk_idcli_ foreign key (idcli) references tbcliente(idcli);

alter table tbcliente add constraint fk_cep foreign key (cep) references tbendereco(cep);

-- datavenda current(defaut())

alter table tbvenda add constraint fk_idcli foreign key (idcli) references tbcliente(idcli);
alter table tbvenda add constraint fk_nf foreign key (nf) references tbnotaFiscal(nf);

alter table tbitemvenda add constraint fk_numerovenda foreign key (numerovenda) references tbvenda(numerovenda);
alter table tbitemvenda add constraint fk_codigobarras_ foreign key (codigobarras) references tbproduto(codigobarras);

alter table tbcompra add constraint fk_codigo foreign key (codigo) references tbfornecedor(codigo);

alter table tbitemcompra add constraint fk_notafiscal foreign key (notafiscal) references tbcompra(notafiscal);
alter table tbitemcompra add constraint fk_codigobarras foreign key (codigobarras) references tbproduto(codigobarras);

alter table tbendereco add constraint fk_idbairro foreign key (idbairro) references tbbairro(idbairro);
alter table tbendereco add constraint fk_idcidade foreign key (idcidade) references tbcidade(idcidade);
alter table tbendereco add constraint fk_iduf foreign key (iduf) references tbestado(iduf);

/*  */

insert into tbfornecedor(codigo, nomefornecedor, cnpj, telfornecedor)  values(1, "Revenda Chico Loco", 1245678937123, 11934567897);
insert into tbfornecedor(codigo, nomefornecedor, cnpj, telfornecedor)  values(2, "José Faz Tudo S/A", 1345678937123, 11934567898);
insert into tbfornecedor(codigo, nomefornecedor, cnpj, telfornecedor)  values(3, "Valdatas Entregas", 1445678937123, 11934567899);
insert into tbfornecedor(codigo, nomefornecedor, cnpj, telfornecedor)  values(4, "Astrolgido das Estrelas", 1545678937123, 11934567800);
insert into tbfornecedor(codigo, nomefornecedor, cnpj, telfornecedor)  values(5, "Amoroso e Doce", 1645678937123, 11934567801);
insert into tbfornecedor(codigo, nomefornecedor, cnpj, telfornecedor)  values(6, "Marcelo dedal", 1745678937123, 11934567802);
insert into tbfornecedor(codigo, nomefornecedor, cnpj, telfornecedor)  values(7, "Franciscano Cachaça", 1845678937123, 11934567803);
insert into tbfornecedor(codigo, nomefornecedor, cnpj, telfornecedor)  values(8, "Joãozinho Chupeta", 1945678937123, 11934567804);

delimiter $$
create procedure spInsertCidade(vIdCidade int, vCidade varchar(200))
begin
	insert into tbcidade(idcidade, cidade) values (vIdCidade, vCidade);
end
$$

call spInsertCidade(1, "Rio de Janeiro");
call spInsertCidade(2, "São Carlos");
call spInsertCidade(3, "Campinas");
call spInsertCidade(4, "Franco da Rocha");
call spInsertCidade(5, "Osasco");
call spInsertCidade(6, "Pirituba");
call spInsertCidade(7, "Lapa");
call spInsertCidade(8, "Ponta Grossa");

select * from tbcidade;

delimiter $$
create procedure spInsertUF(vIdUF int, UF varchar(200))
begin
	insert into tbestado(iduf, uf) values (vIdUF, UF);
end
$$

call spInsertUF(1, "SP");
call spInsertUF(2, "RJ");
call spInsertUF(3, "RS");

select * from tbestado;

delimiter $$
create procedure spInsertBairro(vIdBairro int, Bairro varchar(200))
begin
	insert into tbbairro(idbairro, bairro) values (vIdBairro, Bairro);
end
$$

call spInsertBairro(1, "Aclimação");
call spInsertBairro(2, "Capão Redondo");
call spInsertBairro(3, "Pirituba");
call spInsertBairro(4, "Liberdade");

select * from tbbairro;

delimiter $$
create procedure spInsertProdutos(vCodigodeBarras char(14), vNomeProd varchar(200), vPrecounitarioprod decimal(7,2), vQtd int)
begin
	insert into tbproduto(codigobarras, nomeprod, precounitarioprod, qtd) values (vCodigodeBarras, vNomeProd, vPrecounitarioprod, vQtd);
end
$$

call spInsertProdutos(12345678910111, "Rei de Papel Mache", 54.61, 120);
call spInsertProdutos(12345678910112, "Bolinha de Sabão", 100.45, 120);
call spInsertProdutos(12345678910113, "Carro Bate Bate", 44.00, 120);
call spInsertProdutos(12345678910114, "Bola Furada", 10.00, 120);
call spInsertProdutos(12345678910115, "Maça Laranja", 99.44, 120);
call spInsertProdutos(12345678910116, "Boneco do Hitler", 124.00, 200);
call spInsertProdutos(12345678910117, "Farinha de Surui", 50.00, 200);
call spInsertProdutos(12345678910118, "Zelador de Cemiterio", 24.50, 100);

select * from tbproduto;
show tables;

select * from tbfornecedor;

