---
title: 'CODE-ANALYSIS:7-get-connection-info'
date: 2022-06-12 14:23:31
tags: code analysis, Wi-Fi, Driver;
---

Sat Jun  4 15:03:47 CST 2022

	.get_station = woal_cfg80211_get_station,


// Request the driver to get the station info
static int
woal_cfg80211_get_station(struct wiphy *wiphy, struct net_device *dev,
			  const u8 *mac, struct station_info *sinfo)

// if media is not connected, return directly with ENOENT


	if (MLAN_STATUS_SUCCESS != woal_cfg80211_dump_station_info(priv, sinfo)) {

// request the driver to dump the station info
istatic mlan_status
woal_cfg80211_dump_station_info(moal_private *priv, struct station_info *sinfo)


// get the signal info from firmware
	if (MLAN_STATUS_SUCCESS !=
		    woal_get_signal_info(priv, MOAL_IOCTL_WAIT, &signal)) {

// Get RSSI info
mlan_status
woal_get_signal_info(moal_private *priv, t_u8 wait_option,
		     mlan_ds_get_signal *signal)

/* Fill request buffer */
info = (mlan_ds_get_info *)req->pbuf;
info->sub_command = MLAN_OID_GET_SIGNAL;
info->param.signal.selector = ALL_RSSI_INFO_MASK;
req->req_id = MLAN_IOCTL_GET_INFO;
req->action = MLAN_ACT_GET;

// get stats info from firmware
	if (MLAN_STATUS_SUCCESS !=
		    woal_get_stats_info(priv, MOAL_IOCTL_WAIT, &stats)) {

// get bss info from firmware
	ret = woal_get_bss_info(priv, MOAL_IOCTL_WAIT, &bss_info);

// get DTIM period from firmware
	ret = woal_set_get_dtim_period(priv, MLAN_ACT_GET, MOAL_IOCTL_WAIT,
					       &dtim_period);

// file station info, request the driver to fill the tx/rx rate info, request from the firmware.
woal_cfg80211_fill_rate_info(priv, sinfo);
	rate = (mlan_ds_rate *)req->pbuf;
	rate->sub_command = MLAN_OID_GET_DATA_RATE;
	req->req_id = MLAN_IOCTL_RATE;
	req->action = MLAN_ACT_GET;
	ret = woal_request_ioctl(priv, req, MOAL_IOCTL_WAIT);
