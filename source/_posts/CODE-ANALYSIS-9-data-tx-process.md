---
title: 'CODE-ANALYSIS:9-data-tx-process'
date: 2022-06-12 14:25:54
tags: code analysis, Wi-Fi, Driver;
---

Sun Jun  5 12:16:54 CST 2022

mlan_status
woal_init_sta_dev(struct net_device *dev, moal_private *priv)
	dev->netdev_ops = &woal_netdev_ops;


// upper layer send the data by ndo_start_xmit

/** Network device handlers */
const struct net_device_ops woal_netdev_ops = {
	.ndo_open = woal_open,
		.ndo_start_xmit = woal_hard_start_xmit,



// this function handles packet transmission
netdev_tx_t
woal_hard_start_xmit(struct sk_buff * skb, struct net_device * dev)

   	woal_start_xmit(priv, skb);

 static int
 woal_start_xmit(moal_private *priv, struct sk_buff *skb)
  
 // tcp enhance function 
 	if (priv->enable_tcp_ack_enh == MTRUE) {
			ret = woal_process_tcp_ack(priv, pmbuf);

	index = skb_get_queue_mapping(skb);
		status = mlan_send_packet(priv->phandle->pmlan_adapter, pmbuf);


// function to send packet
mlan_status
mlan_send_packet(t_void *padapter, pmlan_buffer pmbuf)

   			wlan_add_buf_bypass_txqueue(pmadapter, pmbuf);

			or

		    /* Transmit the packet */
		    wlan_wmm_add_buf_txqueue(pmadapter, pmbuf);


// The main process
mlan_status
mlan_main_process(t_void *padapter)

			wlan_wmm_process_tx(pmadapter);

// Transmit the highest priority packet awaiting in the WMM Queues
void
wlan_wmm_process_tx(pmlan_adapter pmadapter)


		if (wlan_dequeue_tx_packet(pmadapter))
					break;

// dequeue a packet
static int
wlan_dequeue_tx_packet(pmlan_adapter pmadapter)


     wlan_11n_aggregate_pkt(priv, ptr, priv->intf_hr_len,
	 					       ptrindex);


pmbuf_src =
		(pmlan_buffer)util_dequeue_list(pmadapter->pmoal_handle,
						&pra_list->buf_head,
						MNULL, MNULL);

    		ret = pmadapter->ops.host_to_card(priv, MLAN_TYPE_DATA,
									  pmbuf_aggr, &tx_param);




// after this I am reading the Wi-Fi subsystem Linux porting guide
