include $(TOPDIR)/rules.mk

PKG_NAME:=pihole
PKG_VERSION:=1.5
PKG_RELEASE:=1

PKG_SOURCE_VERSION:=d885e92674e8d8d9a673b35ae706b2c49ea05840
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/pi-hole/pi-hole.git
PKG_SOURCE:=$(PKG_SOURCE_VERSION).tar.gz
PKG_MIRROR_HASH:=20aa9e6709008025245d41aed0808ba77e6ec3d33a043608889a5cda41048f59


PKG_LICENSE:=EUPL
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)/PIHOLE-$(PKG_SOURCE_VERSION)

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/pihole
	SECTION:=net
	CATEGORY:=Pihole
	TITLE:=A black hole for Internet advertisements
	URL:=https://pi-hole.net/
  	DEPENDS:=pihole-FTL-full pihole-web +sudo +bash +bind-dig +curl +idn +logrotate +coreutils
endef

define Package/pihole/description
	The Pi-hole® is a DNS sinkhole that protects your devices from unwanted content, without installing any client-side software.
endef

define Package/pihole/conffiles
	/etc/pihole
endef

define Package/pihole/install
	$(INSTALL_DIR) $(1)/etc/.pihole
	$(CP) -r $(PKG_BUILD_DIR)/files/* $(1)/etc/.pihole

	$(INSTALL_DIR) $(1)/etc/pihole
	$(INSTALL_DATA) ./files/setupVars.conf $(1)/etc/pihole/
	
	echo -n "" > $(1)/etc/pihole/custom.list
	echo -n "openwrt openwrt " > $(1)/etc/pihole/localbranches
	echo -n "v$(PKG_VERSION)-openwrt v$(PKG_VERSION)-openwrt v$(PKG_VERSION)-openwrt" > $(1)/etc/pihole/localversions
	touch $(1)/etc/pihole/GitHubVersions
endef

define Package/pihole/postinst
#!/bin/sh
if [ -z "$${IPKG_INSTROOT}" ]; then
	service dnsmasq stop
	service dnsmasq disable
	exec /etc/.pihole/automated\ install/basic-install.sh
fi
endef

define Package/pihole/prerm
#!/bin/sh
if [ -z "$${IPKG_INSTROOT}" ]; then
	exec /etc/.pihole/automated\ install/uninstall.sh
fi
endef

define Build/Prepare
	(echo "Starting prep phase")
	$(Build/Prepare/Default)
	(mkdir -p $(PKG_BUILD_DIR)/files/)
	(cp -R $(PKG_BUILD_DIR)/../pihole-$(PKG_VERSION)/* $(PKG_BUILD_DIR)/files/)
	(cd $(PKG_BUILD_DIR)/files/ && git apply /home/builder/pihole-wrt/net/pihole/manual_patches/0001-Update-Install-Script.patch)
	
endef

define Build/Compile
	(echo "Required")
endef

define Build/Install
	(echo "Required")
endef


$(eval $(call BuildPackage,pihole))
