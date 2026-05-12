This solution relies on [Univention UCS](https://www.univention.com/products/ucs/)

# Cisco Side

### AAA Config
<pre>
aaa new-model
!
radius server FREERADIUS
 address ipv4 192.168.1.10 auth-port 1812 acct-port 1813
 key StrongSharedSecret123
!
aaa authentication login default group radius local
!
aaa authorization exec default group radius local
!
line vty 0 15
 login authentication default
 transport input ssh
</pre>

# Univention UCS Side

### LDAP - Create the Groups
<pre>
eval $(ucr shell)

udm groups/group create \
  --position "cn=groups,$ldap_base" \
  --set name="RADIUS Network Admins" \
  --set description="Full RADIUS access to network devices"

udm groups/group create \
  --position "cn=groups,$ldap_base" \
  --set name="RADIUS Network Read Only" \
  --set description="Read-only RADIUS access to network devices"
</pre>

### LDAP - Verifying the groups
<pre>
 udm groups/group list --filter name="RADIUS Network Admins"
 
 udm groups/group list --filter name="RADIUS Network Read Only"
</pre>

### Add users
<pre>
udm groups/group modify \
  --dn "cn=RADIUS Network Admins,cn=groups,$ldap_base" \
  --append users="uid=ariadne,cn=users,$ldap_base"
</pre>

### Verify Users
```
udm users/user list --filter uid=ariadne | grep -i group
```

### FreeRADIUS Clients

<pre>
cat >> /etc/freeradius/3.0/clients.conf << 'EOF'

client management_subnet_52 {
    ipaddr   = 0.0.0.0/24
    secret   = StrongSharedSecret123
    nas-type = cisco
}
EOF
</pre>

### FreeRADIUS Cisco AV Pairs

<pre>
eval $(ucr shell)

cat >> /etc/freeradius/3.0/mods-config/files/authorize << EOF
DEFAULT Ldap-Group == "cn=RADIUS Network Admins,cn=groups,$ldap_base"
        Service-Type = NAS-Prompt-User,
        cisco-avpair = "shell:priv-lvl=15"

DEFAULT Ldap-Group == "cn=RADIUS Network Read Only,cn=groups,$ldap_base"
        Service-Type = NAS-Prompt-User,
        cisco-avpair = "shell:priv-lvl=1"

DEFAULT Auth-Type := Reject
        Reply-Message = "Not in any authorized group"
EOF
</pre>

### Users

Users need to be added to this group directly.


# Testing on Cisco

radtest <user-in-ldap> <ldap-password> <server-ip> 0 <FreeRADIUS-secret>

# Testing On UCS

### Do packets arrive
```
tcpdump -i any -n udp port 1812
```

 