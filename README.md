# controle_veiculo
controle_veiculo

Tecnologia que serão usadas:

SQL Power Architec
Azure
AWS
PHP
Memcached
Mongodb
MySQL
API Rest
Front End Angular, Vuejs ou React
Mobile React
Docker

## Código de Criação em MySQL





CREATE TABLE categoria_endereco (
                categoria_end_id INT AUTO_INCREMENT NOT NULL,
                categoria_end_nome VARCHAR(30) NOT NULL,
                PRIMARY KEY (categoria_end_id)
);

ALTER TABLE categoria_endereco COMMENT 'categoria_endereco';


CREATE TABLE estado_sigla (
                estado_id INT NOT NULL,
                estado_sigla VARCHAR(2) NOT NULL,
                PRIMARY KEY (estado_id)
);

ALTER TABLE estado_sigla COMMENT 'estado_sigla';


CREATE TABLE cidade_nome (
                cidade_id INT AUTO_INCREMENT NOT NULL,
                estado_id INT NOT NULL,
                cidade_nome VARCHAR(30) NOT NULL,
                PRIMARY KEY (cidade_id, estado_id)
);

ALTER TABLE cidade_nome COMMENT 'cidade_nome';


CREATE TABLE endereco_obra (
                enderco_id INT AUTO_INCREMENT NOT NULL,
                estado_id INT NOT NULL,
                endereco_obra VARCHAR(30) NOT NULL,
                endereco_numero INT NOT NULL,
                categoria_end_id INT NOT NULL,
                endereco_observacao VARCHAR(255) NOT NULL,
                PRIMARY KEY (enderco_id)
);

ALTER TABLE endereco_obra COMMENT 'endereco_obra';


CREATE TABLE permissao_acesso (
                permissao_id INT AUTO_INCREMENT NOT NULL,
                permissao_nome VARCHAR(15) NOT NULL,
                PRIMARY KEY (permissao_id)
);

ALTER TABLE permissao_acesso COMMENT 'permissao_acesso';


CREATE TABLE usuario_acesso (
                usuario_id INT NOT NULL,
                permissao_id INT NOT NULL,
                usuario_nome VARCHAR(30) NOT NULL,
                usuario_email VARCHAR(30) NOT NULL,
                usuario_senha VARCHAR(15) NOT NULL,
                PRIMARY KEY (usuario_id, permissao_id)
);

ALTER TABLE usuario_acesso COMMENT 'usuario_acesso';


CREATE TABLE categoria_veiculo (
                categoria_id INT AUTO_INCREMENT NOT NULL,
                categoria_nome VARCHAR(15) NOT NULL,
                PRIMARY KEY (categoria_id)
);

ALTER TABLE categoria_veiculo COMMENT 'categoria_veiculo';


CREATE TABLE verificacao_veiculo (
                verificacao_id INT AUTO_INCREMENT NOT NULL,
                verificacao_concluida BOOLEAN NOT NULL,
                PRIMARY KEY (verificacao_id)
);

ALTER TABLE verificacao_veiculo COMMENT 'verificacao_veiculo';


CREATE TABLE validacao_inspecao (
                validacao_id INT AUTO_INCREMENT NOT NULL,
                validacao BOOLEAN NOT NULL,
                PRIMARY KEY (validacao_id)
);

ALTER TABLE validacao_inspecao COMMENT 'validacao_inspecao';


CREATE TABLE veiculo (
                veiculo_id INT AUTO_INCREMENT NOT NULL,
                verificacao_id INT NOT NULL,
                veiculo_nome VARCHAR(15) NOT NULL,
                veiculo_placa VARCHAR(15) NOT NULL,
                categoria_id INT NOT NULL,
                veiculo_foto_placa LONGBLOB NOT NULL,
                PRIMARY KEY (veiculo_id, verificacao_id)
);

ALTER TABLE veiculo COMMENT 'veiculo';


CREATE TABLE controle_obra (
                obra_id INT NOT NULL,
                veiculo_id INT NOT NULL,
                verificacao_id INT NOT NULL,
                obra_nome VARCHAR(30) NOT NULL,
                enderco_id INT NOT NULL,
                PRIMARY KEY (obra_id, veiculo_id, verificacao_id)
);


CREATE TABLE inspecao (
                inspecao_id INT AUTO_INCREMENT NOT NULL,
                validacao_id INT NOT NULL,
                inspecao_nome VARCHAR(15) NOT NULL,
                inspecao_data DATE NOT NULL,
                veiculo_id INT NOT NULL,
                PRIMARY KEY (inspecao_id, validacao_id)
);

ALTER TABLE inspecao COMMENT 'inspecao_veiculo';


CREATE TABLE motorista (
                motorista_id INT NOT NULL,
                veiculo_id INT NOT NULL,
                motorista_nome VARCHAR(30) NOT NULL,
                PRIMARY KEY (motorista_id, veiculo_id)
);

ALTER TABLE motorista COMMENT 'motorista';


CREATE TABLE controle_saida (
                saida_id INT AUTO_INCREMENT NOT NULL,
                saida_data DATE NOT NULL,
                saida_hora TIME NOT NULL,
                saida_quilometragem TEXT(30) NOT NULL,
                veiculo_id INT NOT NULL,
                assinatura_saida VARCHAR(30) NOT NULL,
                PRIMARY KEY (saida_id)
);

ALTER TABLE controle_saida COMMENT 'controle_saida';


CREATE TABLE controle_entrada (
                entrada_id INT AUTO_INCREMENT NOT NULL,
                entrada_data DATE NOT NULL,
                entrada_hora TIME NOT NULL,
                entrada_quilometragem TEXT(30) NOT NULL,
                veiculo_id INT NOT NULL,
                assinatura_entrada VARCHAR(30) NOT NULL,
                PRIMARY KEY (entrada_id)
);

ALTER TABLE controle_entrada COMMENT 'controle_entrada';


ALTER TABLE endereco_obra ADD CONSTRAINT categoria_endereco_endereco_obra_fk
FOREIGN KEY (categoria_end_id)
REFERENCES categoria_endereco (categoria_end_id)
ON DELETE NO ACTION
ON UPDATE NO ACTION;

ALTER TABLE endereco_obra ADD CONSTRAINT estado_sigla_endereco_obra_fk
FOREIGN KEY (estado_id)
REFERENCES estado_sigla (estado_id)
ON DELETE NO ACTION
ON UPDATE NO ACTION;

ALTER TABLE cidade_nome ADD CONSTRAINT estado_sigla_cidade_nome_fk
FOREIGN KEY (estado_id)
REFERENCES estado_sigla (estado_id)
ON DELETE NO ACTION
ON UPDATE NO ACTION;

ALTER TABLE controle_obra ADD CONSTRAINT endereco_obra_controle_obra_fk
FOREIGN KEY (enderco_id)
REFERENCES endereco_obra (enderco_id)
ON DELETE NO ACTION
ON UPDATE NO ACTION;

ALTER TABLE usuario_acesso ADD CONSTRAINT permissao_acesso_usuario_acesso_fk
FOREIGN KEY (permissao_id)
REFERENCES permissao_acesso (permissao_id)
ON DELETE NO ACTION
ON UPDATE NO ACTION;

ALTER TABLE veiculo ADD CONSTRAINT categoria_veiculo_veiculo_fk
FOREIGN KEY (categoria_id)
REFERENCES categoria_veiculo (categoria_id)
ON DELETE CASCADE
ON UPDATE NO ACTION;

ALTER TABLE veiculo ADD CONSTRAINT verificacao_veiculo_veiculo_fk
FOREIGN KEY (verificacao_id)
REFERENCES verificacao_veiculo (verificacao_id)
ON DELETE NO ACTION
ON UPDATE NO ACTION;

ALTER TABLE inspecao ADD CONSTRAINT validacao_inspecao_inspecao_fk
FOREIGN KEY (validacao_id)
REFERENCES validacao_inspecao (validacao_id)
ON DELETE CASCADE
ON UPDATE NO ACTION;

ALTER TABLE controle_entrada ADD CONSTRAINT veiculo_controle_entrada_fk
FOREIGN KEY (veiculo_id)
REFERENCES veiculo (veiculo_id)
ON DELETE CASCADE
ON UPDATE NO ACTION;

ALTER TABLE controle_saida ADD CONSTRAINT veiculo_controle_saida_fk
FOREIGN KEY (veiculo_id)
REFERENCES veiculo (veiculo_id)
ON DELETE CASCADE
ON UPDATE NO ACTION;

ALTER TABLE motorista ADD CONSTRAINT veiculo_motorista_fk
FOREIGN KEY (veiculo_id)
REFERENCES veiculo (veiculo_id)
ON DELETE CASCADE
ON UPDATE NO ACTION;

ALTER TABLE inspecao ADD CONSTRAINT veiculo_inspecao_fk
FOREIGN KEY (veiculo_id)
REFERENCES veiculo (veiculo_id)
ON DELETE CASCADE
ON UPDATE NO ACTION;

ALTER TABLE controle_obra ADD CONSTRAINT veiculo_controle_obra_fk
FOREIGN KEY (veiculo_id, verificacao_id)
REFERENCES veiculo (veiculo_id, verificacao_id)
ON DELETE CASCADE
ON UPDATE CASCADE;

## 
