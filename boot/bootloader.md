#Locking

##Unlocking

Unlock via `fastboot` for devices that allow it.

> In	some	cases,	like	most	devices	built	for	use	on	the	Verizon	network,	the	ability	for	a	user	to	
unlock	the	bootloader	is	blocked	entirely.	[SamDunk!][SamDunk]

##Security implications

- [Security risks of unlocking](http://android.stackexchange.com/questions/36830/whats-the-security-implication-of-having-an-unlocked-boot-loader)

##Checking bootloader lock status

AFAIK there is no single and/or public apis/methods to check the bootloader lock status. OEMs have different private approaches for doing this. 

See [SamDunk!][SamDunk] for some mention of this and a related Samsung vuln.

> Manufacturers	employ	a	wide	variety	of	methods	and	mechanisms	to	determine	and	control	a
device’s	bootloader	lock	status.		Motorola	uses	TrustZone	protected	fuses,	HTC	uses	data	in	a	
write	protected region	of	eMMC,	and	some	LG	devices	use	a	signed	blob	in	the	boot	(kernel)	
partition.		There	are	likely	many	other	methods,	but	the	concept	is	the	same:	only	allow	the
unlock	status	to	be	changed	via	a	controlled	and	deliberate	method.	

  [SamDunk]: http://theroot.ninja/disclosures/SAMDUNK_1.0-03262016.pdf
