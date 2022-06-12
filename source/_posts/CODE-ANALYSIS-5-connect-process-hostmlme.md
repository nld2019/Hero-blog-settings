---
title: 'CODE-ANALYSIS:5-connect-process-hostmlme'
date: 2022-06-12 14:19:41
tags: code analysis, Wi-Fi, Driver;
---

Sat Jun  4 22:29:17 CST 2022

woal_cfg80211_ops.auth = woal_cfg80211_authenticate;

// This function is authentication handler when host MLME enable
// In this case, driver will prepare and send Auth Req.
static int
woal_cfg80211_authenticate(struct wiphy *wiphy,
			   struct net_device *dev, struct cfg80211_auth_request *req)



// reset cfg_disconnect flag

/** cancel pending scan */
		woal_cancel_scan(priv, MOAL_IOCTL_WAIT);


	/* Not allowed to connect to the same AP which is already connected
		   with other interface */


// create auth frame and fill the info


PRINTM(MCMND, "wlan: HostMlme %s send auth to bssid " MACSTR "\n",
	       dev->name, MAC2STR(req->bss->bssid));



	status = mlan_send_packet(priv->phandle->pmlan_adapter, pmbuf);

// Function to send packet
mlan_status
mlan_send_packet(t_void *padapter, pmlan_buffer pmbuf)


wlan_add_buf_bypass_txqueue(pmadapter, pmbuf);

// Add packet to bypass tx queue
t_void
wlan_add_buf_bypass_txqueue(mlan_adapter *pmadapter, pmlan_buffer pmbuf)
		pmadapter->bypass_pkt_count++;
		util_enqueue_list_tail(pmadapter->pmoal_handle, &priv->bypass_txq,
						(pmlan_linked_list)pmbuf, MNULL, MNULL);


===== Another work queue to process the above bypass_txq
// The main process
mlan_status
mlan_main_process(t_void *padapter)


			PRINTM(MINFO, "mlan_send_pkt(): deq(bybass_txq)\n");
						wlan_process_bypass_tx(pmadapter);

// Transmit the By-passed packet awaiting in by-pass queue
t_void
wlan_process_bypass_tx(pmlan_adapter pmadapter)


		status = wlan_process_tx(pmadapter->
						priv[pmbuf->
						bss_index],
						pmbuf,
						&tx_param);

// This function checks the conditions and sends packet to device
mlan_status
wlan_process_tx(pmlan_private priv, pmlan_buffer pmbuf, mlan_tx_param *tx_param)



head_ptr = (t_u8 *)priv->ops.process_txpd(priv, pmbuf);


static mlan_operations mlan_sta_ops = {
		/* txpd handler */
		wlan_ops_sta_process_txpd,

// This function fill the txpd for tx packet
t_void *
wlan_ops_sta_process_txpd(t_void *priv, pmlan_buffer pmbuf)


// this function send the data to card
		ret = pmadapter->ops.host_to_card(priv, MLAN_TYPE_DATA, pmbuf,
								  tx_param);

	.host_to_card = wlan_sdio_host_to_card_ext,

// This function send data to card
static mlan_status
wlan_sdio_host_to_card_ext(pmlan_private pmpriv, t_u8 type,
			   mlan_buffer *pmbuf, mlan_tx_param *tx_param)


====== above is the auth frame send by the STA

===== below receive auth response from ex-ap

// The main process
mlan_status
mlan_main_process(t_void *padapter)

/* Handle pending interrupts if any */
if (pmadapter->ireg) {
		if (pmadapter->hs_activated == MTRUE)
				wlan_process_hs_config(pmadapter);
		pmadapter->ops.process_int_status(pmadapter);

	.process_int_status = wlan_process_sdio_int_status,




// This function checks the interrupt status and handle it accordingly.
static mlan_status
wlan_process_sdio_int_status(mlan_adapter *pmadapter)


static mlan_status
wlan_decode_rx_packet(mlan_adapter *pmadapter,
		      mlan_buffer *pmbuf, t_u32 upld_typ, t_u8 lock_flag)


// This function processed the received buffer
mlan_status
wlan_handle_rx_packet(pmlan_adapter pmadapter, pmlan_buffer pmbuf)

	ret = priv->ops.process_rx_packet(pmadapter, pmbuf);

wlan_ops_sta_process_rx_packet,

// This function processed the received buffer
mlan_status
wlan_ops_sta_process_rx_packet(t_void *adapter, pmlan_buffer pmbuf)

// This function processes the 802.11 mgmt Frame
mlan_status
wlan_process_802dot11_mgmt_pkt(mlan_private *priv,
			       t_u8 *payload, t_u32 payload_len, RxPD *prx_pd)

    case SUBTYPE_AUTH:
    unicast = MTRUE;
    PRINTM_NETINTF(MMSG, priv);
    PRINTM(MMSG, "wlan: HostMlme Auth received from " MACSTR "\n",
    				MAC2STR(pieee_pkt_hdr->addr2));
    break;



		pevent->event_id = MLAN_EVENT_ID_DRV_MGMT_FRAME;

	wlan_recv_event(priv, pevent->event_id, pevent);

	case MLAN_EVENT_ID_DRV_MGMT_FRAME:

	PRINTM(MEVENT,
					"HostMlme %s: Received auth frame type = 0x%x\n",
					priv->netdev->name,
					priv->auth_alg);


	queue_work
			(priv->
			 phandle->
			 evt_workqueue,
			 &priv->
			 phandle->
			 host_mlme_work);

	MLAN_INIT_WORK(&handle->host_mlme_work, woal_host_mlme_work_queue);

// This workqueue function handles association response in host mlme
void
woal_host_mlme_work_queue(struct work_struct *work)

PRINTM(MCMND, "wlan: HostMlme %s auth success\n",
						       priv->netdev->name);






// After auth success, the upper layer send associate request
woal_cfg80211_ops.assoc = woal_cfg80211_associate;

// This function is association handler when host MLME enable
// In this case driver will prepare and assoc req
static int
woal_cfg80211_associate(struct wiphy *wiphy, struct net_device *dev,
			struct cfg80211_assoc_request *req)

	PRINTM(MCMND, "wlan: HostMlme %s send assoicate to bssid " MACSTR "\n",
		       priv->netdev->name, MAC2STR(req->bss->bssid));


	if (MLAN_STATUS_SUCCESS !=
		    woal_bss_start(priv, MOAL_IOCTL_WAIT_TIMEOUT, ssid_bssid)) {


	/* Fill request buffer */
		bss = (mlan_ds_bss *)req->pbuf;
	
			bss->sub_command = MLAN_OID_BSS_START;
			if (ssid_bssid)
					moal_memcpy_ext(priv->phandle, &bss->param.ssid_bssid,
									ssid_bssid, sizeof(mlan_ssid_bssid),
									sizeof(mlan_ssid_bssid));
			req->req_id = MLAN_IOCTL_BSS;
			req->action = MLAN_ACT_SET;



	/* Send IOCTL request to MLAN */
		status = woal_request_ioctl(priv, req, wait_option);


// send ioctl request to MLAN
mlan_status
woal_request_ioctl(moal_private *priv, mlan_ioctl_req *req, t_u8 wait_option)




	status = mlan_ioctl(priv->phandle->pmlan_adapter, req);




mlan_status
mlan_ioctl(t_void *adapter, pmlan_ioctl_req pioctl_req)
   	ret = pmpriv->ops.ioctl(adapter, pioctl_req);


	wlan_ops_sta_ioctl,



mlan_status
wlan_ops_sta_ioctl(t_void *adapter, pmlan_ioctl_req pioctl_req)
  	case MLAN_IOCTL_BSS:
			status = wlan_bss_ioctl(pmadapter, pioctl_req);
					break;


static mlan_status
wlan_bss_ioctl(pmlan_adapter pmadapter, pmlan_ioctl_req pioctl_req)
	case MLAN_OID_BSS_START:
			status = wlan_bss_ioctl_start(pmadapter, pioctl_req);



// start bss
static mlan_status
wlan_bss_ioctl_start(pmlan_adapter pmadapter, pmlan_ioctl_req pioctl_req)


// find bssid in scan table list


			ret = wlan_associate(pmpriv, pioctl_req,
								     &pmadapter->pscan_table[i]);



// associate to specific bss disconvered in a scan
mlan_status
wlan_associate(mlan_private *pmpriv, t_void *pioctl_buf,
	       BSSDescriptor_t *pbss_desc)



    /** Host Command ID : 802.11 associate */
	#define HostCmd_CMD_802_11_ASSOCIATE 0x0012
	ret = wlan_prepare_cmd(pmpriv, HostCmd_CMD_802_11_ASSOCIATE,
				       HostCmd_ACT_GEN_SET, 0, pioctl_buf, pbss_desc);



ret = pmpriv->ops.prepare_cmd(pmpriv, cmd_no, cmd_action,
							      cmd_oid, pioctl_buf, pdata_buf, cmd_ptr);

			wlan_queue_cmd(pmpriv, pcmd_node, cmd_no);
	

	case HostCmd_CMD_802_11_ASSOCIATE:
			ret = wlan_ret_802_11_associate(pmpriv, resp, pioctl_buf);
					break;


// association firmware response handler
mlan_status
wlan_ret_802_11_associate(mlan_private *pmpriv,
			  HostCmd_DS_COMMAND *resp, t_void *pioctl_buf)


	PRINTM(MCMND, "ASSOC_RESP: %-32s (a_id = 0x%x)\n", pbss_desc->ssid.ssid,
		       wlan_le16_to_cpu(passoc_rsp->a_id));


// now it is simimlar with the no hostmlme type

	/* Send a Media Connected event, according to the Spec */
		pmpriv->media_connected = MTRUE;


	wlan_recv_event(pmpriv, MLAN_EVENT_ID_DRV_CONNECTED, pevent);


    wlan_recv_event(pmpriv, MLAN_EVENT_ID_DRV_ASSOC_SUCC_LOGGER, pevent);


	.moal_recv_event = moal_recv_event,


// This function handle event received
mlan_status
moal_recv_event(t_void *pmoal, pmlan_event pmevent)


    woal_broadcast_event(priv, pmevent->event_buf,
				pmevent->event_len);
	woal_update_dscp_mapping(priv);
	priv->media_connected = MTRUE;


===============// 4-way handshake

// The main process
mlan_status
mlan_main_process(t_void *padapter)

/* Handle pending interrupts if any */
if (pmadapter->ireg) {
		if (pmadapter->hs_activated == MTRUE)
				wlan_process_hs_config(pmadapter);
		pmadapter->ops.process_int_status(pmadapter);

	.process_int_status = wlan_process_sdio_int_status,


// This function checks the interrupt status and handle it accordingly.
static mlan_status
wlan_process_sdio_int_status(mlan_adapter *pmadapter)



static mlan_status
wlan_decode_rx_packet(mlan_adapter *pmadapter,
		      mlan_buffer *pmbuf, t_u32 upld_typ, t_u8 lock_flag)

mlan_status
wlan_handle_rx_packet(pmlan_adapter pmadapter, pmlan_buffer pmbuf)

	PRINTM(MDATA, "%lu.%06lu : Data <= FW\n", sec, usec);
		ret = priv->ops.process_rx_packet(pmadapter, pmbuf);

	/* rx handler */
		wlan_ops_sta_process_rx_packet,

// This function processes the received buffer
mlan_status
wlan_ops_sta_process_rx_packet(t_void *adapter, pmlan_buffer pmbuf)



// this function process the received packet and forwards it to kernel/upper layer
mlan_status
wlan_process_rx_packet(pmlan_adapter pmadapter, pmlan_buffer pmbuf)

	ret = pmadapter->callbacks.moal_recv_packet(pmadapter->pmoal_handle,
							    pmbuf);
	
    pmadapter->ops.data_complete(pmadapter, pmbuf, ret);

// this fucntion uploads the packet to network stack
mlan_status
moal_recv_packet(t_void *pmoal, pmlan_buffer pmbuf)

	if (ntohs(ethh->h_proto) == ETH_P_PAE)
			PRINTM(MEVENT,
							"wlan: %s Rx EAPOL pkt from " MACSTR
							"\n",
							priv->netdev->name,
							MAC2STR(ethh->h_source));



	.moal_recv_packet = moal_recv_packet,

// this fucntion uploads the packet to network stack
mlan_status
moal_recv_packet(t_void *pmoal, pmlan_buffer pmbuf)


				skb_reserve(skb, pmbuf->data_offset);

			    skb_put(skb, pmbuf->data_len);


	netif_rx(skb);
	or
	netif_receive_skb(skb);
	or 
	netif_rx_ni(skb);


// the above process include the process of rx data

