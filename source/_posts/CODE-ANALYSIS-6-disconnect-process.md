---
title: 'CODE-ANALYSIS:6-disconnect-process'
date: 2022-06-12 14:22:25
tags: code analysis, Wi-Fi, Driver;
---

Sat Jun  4 15:46:42 CST 2022

	.disconnect = woal_cfg80211_disconnect,


// request the driver to disconnect
static int
woal_cfg80211_disconnect(struct wiphy *wiphy, struct net_device *dev,
			 t_u16 reason_code)

wlan: Received disassociation request on < interface >, reason: xx


print disconnect reason code


// check current driver_status, 0 indicate normal state, 1 abnormal state;
abnormal state, the Wi-Fi function can't be used.


if already disconnect, process it separately.


	/** cancel pending scan */
		woal_cancel_scan(priv, MOAL_IOCTL_WAIT);


// send deauth command to MLAN
	if (woal_disconnect(priv, MOAL_IOCTL_WAIT_TIMEOUT, priv->cfg_bssid,
				    reason_code) != MLAN_STATUS_SUCCESS) {

// send command to firmware
			/* Fill request buffer */
			bss = (mlan_ds_bss *)req->pbuf;
			bss->sub_command = MLAN_OID_BSS_STOP;
			if (mac)
					moal_memcpy_ext(priv->phandle,
									&bss->param.deauth_param.mac_addr, mac,
									sizeof(mlan_802_11_mac_addr),
									sizeof(bss->param.deauth_param.mac_addr));
			bss->param.deauth_param.reason_code = reason_code;
			req->req_id = MLAN_IOCTL_BSS;
			req->action = MLAN_ACT_SET;



// notify cfg80211 that connection was dropped.
// After it calls this function, the driver should enter 
// an idle state and not try to connect to any AP any more.
cfg80211_disconnected(priv->netdev, 0, NULL, 0, true, GFP_KERNEL);


clear connection parameters


