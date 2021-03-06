---
title: 'CODE-ANALYSIS:4-scan-process'
date: 2022-06-12 14:17:46
tags: code analysis, Wi-Fi, Driver;
---

Fri Jun  3 20:55:15 CST 2022

// driver scan process.

/** cfg80211 operations */
static struct cfg80211_ops woal_cfg80211_ops = {
		.change_virtual_intf = woal_cfg80211_change_virtual_intf,
		.scan = woal_cfg80211_scan,

* @brief Request the driver to do a scan. Always returning
* zero meaning that the scan request is given to driver,
* and will be valid until passed to cfg80211_scan_done().
* To inform scan results, call cfg80211_inform_bss().
*
static int
woal_cfg80211_scan(struct wiphy *wiphy, struct cfg80211_scan_request *request)

	PRINTM(MINFO, "Received scan request on %s\n", dev->name);	
	
if (MLAN_STATUS_SUCCESS != woal_do_scan(priv, scan_req)) {

// request scan
mlan_status
woal_do_scan(moal_private *priv, wlan_user_scan_cfg *scan_cfg)



if (!scan_cfg)
		ret = woal_request_scan(priv, MOAL_NO_WAIT, NULL);
else
		ret = woal_request_userscan(priv, MOAL_NO_WAIT, scan_cfg);


scan = (mlan_ds_scan *)ioctl_req->pbuf;
scan->sub_command = MLAN_OID_SCAN_USER_CONFIG;
ioctl_req->req_id = MLAN_IOCTL_SCAN;
ioctl_req->action = MLAN_ACT_SET;


	/* Send IOCTL request to MLAN */
		status = woal_request_ioctl(priv, ioctl_req, wait_option);



	status = mlan_ioctl(priv->phandle->pmlan_adapter, req);

static mlan_operations mlan_sta_ops = {
		/* init cmd handler */
		wlan_ops_sta_init_cmd,
		/* ioctl handler */
		wlan_ops_sta_ioctl,
		/* cmd handler */
		wlan_ops_sta_prepare_cmd,


mlan_status
wlan_ops_sta_ioctl(t_void *adapter, pmlan_ioctl_req pioctl_req)


case MLAN_IOCTL_SCAN:
status = wlan_scan_ioctl(pmadapter, pioctl_req);
break;


// Get /set scan
static mlan_status
wlan_scan_ioctl(pmlan_adapter pmadapter, pmlan_ioctl_req pioctl_req)
PRINTM(MMSG, "wlan: %s START SCAN\n", dev->name);

// to start scan based on input config
case MLAN_OID_SCAN_USER_CONFIG:
status = wlan_scan_networks(pmpriv, pioctl_req,
				(wlan_user_scan_cfg *)
				pscan->param.user_scan.
				scan_cfg_buf);
break;

// Construct and send multiple scan config commands to the firmware
	ret = wlan_scan_channel_list(pmpriv, pioctl_buf, max_chan_per_scan,
					     filtered_scan, &pscan_cfg_out->config,
						pchan_list_out, pscan_chan_list);


// Send the scan command to the firmware with the specified cfg
	cmd_no = HostCmd_CMD_802_11_SCAN_EXT;
	else
			cmd_no = HostCmd_CMD_802_11_SCAN;
	ret = wlan_prepare_cmd(pmpriv, cmd_no, HostCmd_ACT_GEN_SET, 0,
					MNULL, pscan_cfg_out);


// 	/* Get scan command from scan_pending_q and put to cmd_pending_q */
	wlan_insert_cmd_to_pending_q(pmadapter,
					pcmd_node, MTRUE);

	PRINTM(MCMND, "QUEUE_CMD: cmd=0x%x is queued\n", command);

// main process
mlan_status
mlan_main_process(t_void *padapter)


if (wlan_exec_next_cmd(pmadapter) ==


// This function execute the next command in the cmd pending queue.
mlan_status
wlan_exec_next_cmd(mlan_adapter *pmadapter)



util_unlink_list(pmadapter->pmoal_handle,
		&pmadapter->cmd_pending_q,
		(pmlan_linked_list)pcmd_node, MNULL, MNULL);
wlan_release_cmd_lock(pmadapter);
ret = wlan_dnld_cmd_to_fw(priv, pcmd_node);


// This function download a command to firmware
static mlan_status
wlan_dnld_cmd_to_fw(mlan_private *pmpriv, cmd_ctrl_node *pcmd_node)


/* Send the command to lower layer */
ret = pmadapter->ops.host_to_card(pmpriv, MLAN_TYPE_CMD,
				pcmd_node->cmdbuf, MNULL);


mlan_adapter_operations mlan_sdio_ops = {
		.dnld_fw = wlan_sdio_dnld_fw,
		.host_to_card = wlan_sdio_host_to_card_ext,


// This function sends data to card
static mlan_status
wlan_sdio_host_to_card_ext(pmlan_private pmpriv, t_u8 type,
			   mlan_buffer *pmbuf, mlan_tx_param *tx_param)

// This function will send the data to card by methods via the type of the data.(data/ command)
	ret = wlan_sdio_host_to_card(pmadapter, type, pmbuf, tx_param);

/* Setup the timer after transmit command */
pcb->moal_start_timer(pmadapter->pmoal_handle,
				pmadapter->pmlan_cmd_timer, MFALSE, timeout);

==============================================================================================

// This function handles events generated by firmware
mlan_status
wlan_ops_sta_process_event(t_void *priv)


// process scan result through event
case EVENT_EXT_SCAN_REPORT:      // 0x58
PRINTM(MEVENT, "EVENT: EXT_SCAN Report (%d)\n",
				pmbuf->data_len);
if (pmadapter->pscan_ioctl_req && pmadapter->ext_scan)
		ret = wlan_handle_event_ext_scan_report(priv, pmbuf);
break;


// This function handle the event extended scan report
mlan_status
wlan_handle_event_ext_scan_report(mlan_private *pmpriv, mlan_buffer *pmbuf)


// This function parse and sotre the extended scan result
wlan_parse_ext_scan_result(pmpriv, pevent_scan->num_of_set, ptlv,
				   tlv_buf_left);


// This fucnntion handle the event exteded scan status
case EVENT_EXT_SCAN_STATUS_REPORT:      // 0x7f
PRINTM(MEVENT, "EVENT: EXT_SCAN status report (%d)\n",
				pmbuf->data_len);
pmadapter->ext_scan_timeout = MFALSE;
ret = wlan_handle_event_ext_scan_status(priv, pmbuf);
break;


// print current scan status


// update channel statistics from scan result
case TLV_TYPE_CHANNEL_STATS:
tlv_chan_stats = (MrvlIEtypes_ChannelStats_t *)tlv;
wlan_update_chan_statistics(pmpriv, tlv_chan_stats);
break;


/* Now we got response from FW, cancel the command timer */


// process the resulting scan table
wlan_scan_process_results(pmpriv);
// Post process the scan table after a new scan command has completed
static t_void
wlan_scan_process_results(mlan_private *pmpriv)


// check if the media_connected status
// if it is connected, update the status of the current work

// find a specific compatible BSSID in the scan list
j = wlan_find_bssid_in_list(pmpriv,
				pmpriv->curr_bss_params.
				bss_descriptor.mac_address,
				pmpriv->bss_mode);

// save the current beacon buffer
    wlan_save_curr_bcn(pmpriv);

caculate the channel load

update channel rssi



/*
 * Prepares domain info from scan table and downloads the
 *   domain info command to the FW.
 */
wlan_11d_prepare_dnld_domain_info_cmd(pmpriv);
PRINTM(MMSG, "wlan: SCAN COMPLETED: scanned AP count=%d\n",
				pmadapter->num_in_scan_table);


// This function parse country info from AP and download country info to FW
/* Check if connected */
if (pmpriv->media_connected == MTRUE) {
		ret = wlan_11d_parse_dnld_countryinfo(pmpriv,
						&pmpriv->
						curr_bss_params.
						bss_descriptor);
} else {
		ret = wlan_11d_parse_dnld_countryinfo(pmpriv, MNULL);
}


/* Generate domain info */
wlan_11d_generate_domain_info(pmadapter, &region_chan);


/* Set domain info */
ret = wlan_11d_send_domain_info(pmpriv, MNULL);
/* Send cmd to FW to set domain info */
ret = wlan_prepare_cmd(pmpriv, HostCmd_CMD_802_11D_DOMAIN_INFO,
				HostCmd_ACT_GEN_SET, 0, (t_void *)pioctl_buf,
				MNULL);


complete the scan ioctl



