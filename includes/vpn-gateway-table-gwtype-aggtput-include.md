| | **Taxa de transferência de Gateway de VPN (1)** | **Túneis IPsec máximo de Gateway de VPN (2)** | **Taxa de transferência de Gateway de Rota Expressa** | **Coexistência de Gateway de VPN e a Rota Expressa**|
|--- |----------------------------|-----------------------------------|-------------------------------------|-----------------------------------------|
| **SKU Básico** | 100 Mbps | 10 | 500 Mbps | Não |
| **SKU Standard (3)** | 100 Mbps | 10 | 1000 Mbps | Sim |
| **SKU de Alto Desempenho (3)** | 200 Mbps | 30 | 2000 Mbps | Sim |

- (1) A taxa de transferência da VPN é uma estimativa aproximada baseada nas medidas entre redes virtuais na mesma região do Azure. Não é uma taxa de transferência garantida para conexões entre locais na Internet. É a medida de taxa de transferência máxima possível.
- (2) O número de túneis refere-se às VPNs baseadas em rota. Uma VPN PolicyBased só pode dar suporte a um túnel de VPN de Site a Site.
- (3) não há suporte para VPNs PolicyBased neste SKU. Eles recebem suporte apenas no SKU básico.

<!---HONumber=AcomDC_0921_2016-->