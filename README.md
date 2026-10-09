# Cisco Packet Tracer | Network Labs

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer-0B5CAD?style=flat-square)
![Networking](https://img.shields.io/badge/Focus-Networking%20%26%20Security-2563EB?style=flat-square)
![Status](https://img.shields.io/badge/Status-In%20Progress-2EA44F?style=flat-square)

## Sobre o repositório

Este repositório reúne meus laboratórios práticos desenvolvidos no **Cisco Packet Tracer**, com foco em configuração de dispositivos Cisco, fundamentos de redes, segmentação, roteamento e diagnóstico de conectividade.

O objetivo é documentar minha evolução nos estudos de infraestrutura de redes e segurança, aplicando os conceitos em topologias simuladas e reproduzíveis.

## Laboratórios disponíveis

| Laboratório | Conteúdo | Arquivo |
| --- | --- | --- |
| 01 — Inter-VLAN Routing | VLANs, portas access, trunk 802.1Q e Router-on-a-Stick | [Abrir arquivo .pkt](./Inter-VLAN%20Routing%20with%20IEEE%20802.1Q%20Trunking.pkt) |

## Laboratório 01 — Inter-VLAN Routing with IEEE 802.1Q Trunking

### Objetivo

Implementar a comunicação entre duas redes lógicas segmentadas por VLANs, utilizando dois switches Cisco e um roteador para realizar **roteamento inter-VLAN** por meio de subinterfaces.

### Topologia

```text
                 [Router Cisco]
                       |
                trunk 802.1Q
                       |
                   [Switch0]
                   /   |   \
                 PC0  PC1  PC2
                       |
                trunk 802.1Q
                       |
                   [Switch1]
                   /   |   \
                 PC3  PC4  PC5
```

**Dispositivos utilizados:**
- 1 roteador Cisco
- 2 switches Cisco
- 6 computadores
- Conexões Ethernet para hosts e links trunk entre os equipamentos de rede

### Segmentação da rede

| VLAN | Departamento | Sub-rede | Gateway |
| --- | --- | --- | --- |
| 10 | Administrativo | `192.168.10.0/24` | `192.168.10.1` |
| 20 | TI | `192.168.20.0/24` | `192.168.20.1` |

Cada VLAN representa um domínio de broadcast separado. O tráfego entre as VLANs passa pelo roteador.

### Tecnologias aplicadas

- **VLANs (IEEE 802.1Q):** segmentação lógica da rede.
- **Access ports:** associação de portas dos switches a VLANs específicas.
- **Trunking:** transporte de tráfego de múltiplas VLANs pelos enlaces entre dispositivos.
- **Router-on-a-Stick:** uso de subinterfaces na mesma interface física do roteador para realizar roteamento entre VLANs.
- **IPv4 e gateway padrão:** endereçamento e encaminhamento entre sub-redes.
- **ICMP e troubleshooting:** validação de conectividade e identificação de problemas de configuração.

### Exemplos de configuração

**Criação de VLANs no switch:**

```cisco
enable
configure terminal
vlan 10
 name ADMIN
exit
vlan 20
 name TI
exit
```

**Exemplo de configuração de trunk:**

```cisco
interface gigabitEthernet 0/1
 switchport mode trunk
exit
```

**Subinterfaces do roteador:**

```cisco
interface gigabitEthernet 0/0
 no ip address
 no shutdown
exit

interface gigabitEthernet 0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
exit

interface gigabitEthernet 0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
exit
```

> Os trechos acima documentam os conceitos centrais do laboratório. As portas de acesso e os enlaces devem ser conferidos na topologia do arquivo `.pkt`.

### Verificação e diagnóstico

Comandos utilizados nos equipamentos Cisco:

```cisco
show vlan brief
show interfaces trunk
show ip interface brief
show ip route
```

Teste de comunicação entre as redes a partir de um PC da VLAN 10:

```text
ping 192.168.20.2
```

Durante a configuração, foi identificado um problema na subinterface `GigabitEthernet0/0.20`: ela estava operacional, porém sem endereço IPv4. Após atribuir `192.168.20.1/24`, a rede `192.168.20.0/24` passou a constar na tabela de roteamento e o teste de comunicação entre VLANs funcionou.

**Aprendizado principal:** interfaces em estado `up/up` não garantem, por si só, uma configuração IP ou roteamento correto.

## Como executar

1. Instale o **Cisco Packet Tracer**.
2. Baixe o arquivo [Inter-VLAN Routing with IEEE 802.1Q Trunking.pkt](./Inter-VLAN%20Routing%20with%20IEEE%20802.1Q%20Trunking.pkt).
3. Abra o arquivo no Packet Tracer.
4. Consulte as configurações dos switches e do roteador pela aba **CLI**.
5. Utilize **Desktop > Command Prompt** nos computadores para executar testes de `ping`.
6. Explore o **Simulation Mode** para acompanhar o tráfego de rede.

## Próximos estudos

Os temas planejados para novos laboratórios incluem:

- Servidor DHCP e distribuição automática de endereços
- ACLs (Access Control Lists)
- NAT e PAT
- Roteamento estático e dinâmico
- Segurança de switches e segmentação de rede

## Autor

**Vinicius Lopes**  
Estudante de desenvolvimento de software, redes de computadores e cibersegurança.

- [GitHub](https://github.com/Lopes-V)
- [LinkedIn](https://www.linkedin.com/in/lopes-v-dev/)

---

*Laboratórios realizados para fins educacionais, em ambiente simulado com Cisco Packet Tracer.*
