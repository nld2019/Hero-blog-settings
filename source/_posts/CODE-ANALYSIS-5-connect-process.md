---
title: 'CODE-ANALYSIS:5-connect-process'
date: 2022-06-12 14:21:11
tags: code analysis, Wi-Fi, Driver;
---

Sat Jun  4 16:01:49 CST 2022

// connect process


.connect = woal_cfg80211_connect,

* @brief Request the driver to connect to the ESS with
* the specified parameters from kernel
static int
woal_cfg80211_connect(struct wiphy *wiphy, struct net_device *dev,
		      struct cfg80211_connect_params *sme)

received association request on < interface > 

reset cfg_disconnect status

copy SSID and BSSID from the upper layer parameters


check the current status, not allowed to connect the same AP which is already connected with other interface.


	/** cancel pending scan */
		woal_cancel_scan(priv, MOAL_IOCTL_WAIT);

if the connect is triggered by the 11r roaming from supplicant do sperated process.

    get current bss info
	get target bss info


else 
set the cfg_connect status


if current scan type is passive, set it to active.

// request the driver for (re)association
	ret = woal_cfg80211_assoc(priv, (void *)sme, MOAL_IOCTL_WAIT,
					  assoc_rsp);


// send bss start command to mlan
	    woal_bss_start(priv, MOAL_IOCTL_WAIT_TIMEOUT, ssid_bssid)) {

	status = mlan_ioctl(priv->phandle->pmlan_adapter, req);

	ret = pmpriv->ops.ioctl(adapter, pioctl_req);

	wlan_ops_sta_ioctl,

// MLAN station ioctl handler
mlan_status
wlan_ops_sta_ioctl(t_void *adapter, pmlan_ioctl_req pioctl_req)
	case MLAN_IOCTL_BSS:
			status = wlan_bss_ioctl(pmadapter, pioctl_req);
					break;

// BSS command handler
static mlan_status
wlan_bss_ioctl(pmlan_adapter pmadapter, pmlan_ioctl_req pioctl_req)
	case MLAN_OID_BSS_START:
			status = wlan_bss_ioctl_start(pmadapter, pioctl_req);
					break;

// START BSS
static mlan_status
wlan_bss_ioctl_start(pmlan_adapter pmadapter, pmlan_ioctl_req pioctl_req)

			ret = wlan_associate(pmpriv, pioctl_req,
								     &pmadapter->pscan_table[i]);

// associate to specific  BSS disconvered in a scan
mlan_status
wlan_associate(mlan_private *pmpriv, t_void *pioctl_buf, BSSDescriptor_t *pbss_desc)
    	ret = wlan_prepare_cmd(pmpriv, HostCmd_CMD_802_11_ASSOCIATE,
					       HostCmd_ACT_GEN_SET, 0, pioctl_buf, pbss_desc);


// This function prepare the command before sending to firmware.
mlan_status
wlan_prepare_cmd(mlan_private *pmpriv, t_u16 cmd_no,
		 t_u16 cmd_action, t_u32 cmd_oid,
		 		 t_void *pioctl_buf, t_void *pdata_buf)

		ret = pmpriv->ops.prepare_cmd(pmpriv, cmd_no, cmd_action,
							      cmd_oid, pioctl_buf, pdata_buf,
								  					      cmd_ptr);

wlan_ops_sta_prepare_cmd,

mlan_status
wlan_ops_sta_prepare_cmd(t_void *priv, t_u16 cmd_no,
			 t_u16 cmd_action, t_u32 cmd_oid,
			 			 t_void *pioctl_buf,
						 			 t_void *pdata_buf, t_void *pcmd_buf)
	case HostCmd_CMD_802_11_ASSOCIATE:
			ret = wlan_cmd_802_11_associate(pmpriv, cmd_ptr, pdata_buf);
	break;

// This function prepare the command of association
mlan_status
wlan_cmd_802_11_associate(mlan_private *pmpriv,
			  HostCmd_DS_COMMAND *cmd, t_void *pdata_buf)



wlan_queue_cmd(pmpriv, pcmd_node, cmd_no);



// print connected to bssid xxx successfully.
	if (!ret && priv->media_connected) {
			PRINTM(MMSG,
					       "wlan: Connected to bssid " MACSTR " successfully\n",


============================================EVENT ============================ =

// The main process
mlan_status
mlan_main_process(t_void *padapter)

// This function handle the command response
mlan_status
wlan_process_cmdresp(mlan_adapter *pmadapter)


// This function handle the station command response.
mlan_status
wlan_ops_sta_process_cmdresp(t_void *priv, t_u16 cmdresp_no,
			     t_void *pcmd_buf, t_void *pioctl)
	case HostCmd_CMD_802_11_ASSOCIATE:
			ret = wlan_ret_802_11_associate(pmpriv, resp, pioctl_buf);
					break;


// Association firmware command response handler
mlan_status
wlan_ret_802_11_associate(mlan_private *pmpriv,
			  HostCmd_DS_COMMAND *resp, t_void *pioctl_buf)

	/* Send a Media Connected event, according to the Spec */
		pmpriv->media_connected = MTRUE;
	
		PRINTM(MCMND, "ASSOC_RESP: %-32s (a_id = 0x%x)\n", pbss_desc->ssid.ssid,
			       wlan_le16_to_cpu(passoc_rsp->a_id));


    MLAN_EVENT_ID_DRV_CONNECTED = 0x80000001,
	wlan_recv_event(pmpriv, MLAN_EVENT_ID_DRV_CONNECTED, pevent);

	case MLAN_EVENT_ID_DRV_CONNECTED:

    MLAN_EVENT_ID_DRV_ASSOC_SUCC_LOGGER = 0x80000027, 
	wlan_recv_event(pmpriv, MLAN_EVENT_ID_DRV_ASSOC_SUCC_LOGGER, pevent);


	woal_broadcast_event(priv, pmevent->event_buf,
					pmevent->event_len);

// This function handle events generated by firmware
mlan_status
woal_broadcast_event(moal_private *priv, t_u8 *payload, t_u32 len)

  /* Send message */
  ret = netlink_broadcast(sk, skb, 0, NL_MULTICAST_GROUP, GFP_ATOMIC);



	priv->media_connected = MTRUE;


.moal_recv_event = moal_recv_event,

// This function handle event received (MOAL)
mlan_status
moal_recv_event(t_void *pmoal, pmlan_event pmevent)




