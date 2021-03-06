###<a name="does-azure-charge-for-traffic-between-vnets"></a>O Azure cobra o tráfego entre VNets?
O tráfego VNet a VNet na mesma região é gratuito em ambas as direções. O tráfego de saída VNet a VNet entre várias regiões é cobrado com as taxas de transferência de dados inter-VNet de saída baseadas nas regiões de origem. Veja a [página de preços](https://azure.microsoft.com/pricing/details/vpn-gateway/) para obter detalhes.

###<a name="does-vnet-to-vnet-traffic-travel-across-the-internet"></a>O tráfego VNet a VNet viaja em toda a Internet?
Não. O tráfego VNet a VNet circula no backbone do Microsoft Azure e não na Internet.

### <a name="is-vnet-to-vnet-traffic-secure"></a>O tráfego VNet a VNet é seguro?
Sim, está protegido pela encriptação IPsec/IKE.

###<a name="do-i-need-a-vpn-device-to-connect-vnets-together"></a>Preciso de um dispositivo VPN para ligar VNets?
Não. A ligação de várias redes virtuais do Azure em conjunto não precisa de um dispositivo VPN, a menos que precise de conectividade em vários locais.

###<a name="do-my-vnets-need-to-be-in-the-same-region"></a>As minhas VNets precisam de estar na mesma região?
Não. As redes virtuais podem estar nas mesmas regiões ou em regiões (localizações) diferentes do Azure.

###<a name="can-i-use-vnet-to-vnet-along-with-multi-site-connections"></a>Pode utilizar a VNet a VNet juntamente com ligações de vários locais?
Sim. A conectividade de rede virtual pode ser utilizada em simultâneo com VPNs de vários locais.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>A quantos sites no local e redes virtuais pode uma rede virtual ligar?
Consulte a tabela [Requisitos do gateway](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements).

###<a name="can-i-use-vnet-to-vnet-to-connect-vms-or-cloud-services-outside-of-a-vnet"></a>Posso utilizar a VNet a VNet para ligar VMs ou serviços cloud fora de uma VNet?
Não. A VNet a VNet suporta a ligação de redes virtuais. Não suporta a ligação de máquinas virtuais ou serviços cloud que não estiverem numa rede virtual.

###<a name="can-a-cloud-service-or-a-load-balancing-endpoint-span-vnets"></a>Um serviço cloud ou um ponto de final de balanceamento de carga pode abranger VNets?
Não. Um serviço cloud ou um ponto final de balanceamento de carga não pode abranger várias redes virtuais, mesmo se estiverem ligadas em conjunto.

###<a name="can-i-used-a-policybased-vpn-type-for-vnet-to-vnet-or-multi-site-connections"></a>Pode utilizar um tipo de VPN PolicyBased para ligações de VNet a VNet ou de Vários Locais?
Não. As ligações VNet a VNet ou de Vários Sites precisam de gateway de VPN do Azure com tipos de VPN RouteBased (anteriormente denominado Encaminhamento Dinâmico).

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>Posso ligar uma VNet com um Tipo de VPN RouteBased a outra VNet com um tipo de VPN PolicyBased?
Não, ambas as redes virtuais TÊM de utilizar VPNs baseadas na rota (anteriormente chamado Encaminhamento Dinâmico).

###<a name="do-vpn-tunnels-share-bandwidth"></a>Os túneis VPN partilham a largura de banda?
Sim. Todos os túneis VPN da rede virtual partilham a largura de banda disponível no gateway de VPN do Azure e o mesmo SLA do tempo de atividade do gateway de VPN no Azure.

###<a name="are-redundant-tunnels-supported"></a>Os túneis redundantes são suportados?
Os túneis redundantes entre um par de redes virtuais são suportados quando um gateway de rede virtual está configurado como ativo-ativo.

###<a name="can-i-have-overlapping-address-spaces-for-vnet-to-vnet-configurations"></a>Pode ter espaços de endereços sobrepostos para configurações de VNet a VNet?
Não. Não pode ter intervalos de endereços IP sobrepostos.

### <a name="can-there-be-overlapping-address-spaces-among-connected-virtual-networks-and-on-premises-local-sites"></a>Pode existir uma sobreposição de espaços de endereços entre as redes virtuais ligadas e os sites locais no local?
Não. Não pode ter intervalos de endereços IP sobrepostos.





<!--HONumber=Feb17_HO3-->


