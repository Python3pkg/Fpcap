# Welcome to Fpcap
[![Version][version-badge]][version-link] ![Supported-python-version][python27-badge] [![Build Status][travis-badge]][travis-link]  [![Coverage][coverage-badge]][coverage-link] ![Star][stars] ![Fork][forks] [![MIT License][license-badge]](LICENSE.md)

## Introduction

Fpcap is a pure Python library designed to capture RTP on established session of [FREESWITCH](https://freeswitch.org/).
In [/scripts](https://github.com/sudaning/Fpcap/tree/master/scripts) , there are some scripts written by me for daily use.

## Installation
1. Via **pip**  
```pip install pyFpcap```  
2. Via **easy_install**  
```easy_install pyFpcap```  
3. From **source**(recommend)   
```python setup.py install```  

## upgrading
1. Via **pip**  
```pip install --upgrade pyFpcap```

## Examples
```python
#!/usr/bin/env python 
# -*- coding: utf-8 -*- 
import os
from optparse import OptionParser, OptionGroup
from fpcap import capturePcap

if __name__ == '__main__':

	usage = "usage: \n\tpython %prog [options]" + \
		"\nFor example: \n\tpython %prog --num1 18682099276 [-i eth0] [-s 10.9.0.108] [-p 8021] [-a ClueCon]" + \
		"\n\tpython %prog --num1 18682099276 --num2 13798283294 [-i eth0] [-s 10.9.0.108] [-p 8021] [-a ClueCon]" 
	parser = OptionParser(usage=usage, version="%prog V1.0")
	parser.add_option('-s', '--host', dest='host', default='localhost',help="ESL IP. default:'%default'")
	parser.add_option('-p', '--port', dest='port', default='8021', help="ESL port. default:'%default'")
	parser.add_option('-a', '--password', dest='password', default='ClueCon', help="ESL password. default:'%default'")
	
	# 需要抓包的网卡
	parser.add_option('-i', '--eth', dest='eth', default='bond0', help="capture RTP on which eth. default:'%default'")

	group = OptionGroup(parser, "List Options", "monitor numbers(but just only one will be monitored in same time)")
	max_num = 20
	for i in range(1, max_num + 1):
		group.add_option('--num%d' % i, dest='num%d' % i, default='', help="monitor number%d" % i)
	else:
		parser.add_option_group(group)  
	
	(options, args) = parser.parse_args()

	# 至少需要1个号码
	num_list = filter(lambda x: x != '', [eval('options.num%d' % x) for x in range(1, max_num + 1)])
	if not num_list:
		print("no any numbers will be monitored")
		os.sys.exit(0)

	try:
		with capturePcap(options.host, options.port, options.password, debug=False) as p:
			p.set_pcap(eth=options.eth).set_monitor_numbers(num_list)
			print("number monitored list:%s" % num_list)
			p.run(36000)
	except Exception as err:
		print(err)
```

## From the author
**Welcome to use Fpcap (●'◡'●)ﾉ♥**  
If you find any bug, please report it to me by opening a issue.
Fpcap needs to be improved, your contribution will be welcomed.

[version-badge]:   https://img.shields.io/pypi/v/pyFpcap.svg?label=pypi
[version-link]:    https://pypi.python.org/pypi/pyFpcap/
[python27-badge]:  https://img.shields.io/badge/python-2.7-green.svg
[stars]:           https://img.shields.io/github/stars/sudaning/Fpcap.svg
[forks]:           https://img.shields.io/github/forks/sudaning/Fpcap.svg
[travis-badge]:    https://img.shields.io/travis/sudaning/Fpcap.svg
[travis-link]:     https://travis-ci.org/sudaning/Fpcap
[coverage-badge]:  https://img.shields.io/coveralls/sudaning/Fpcap.svg
[coverage-link]:   https://coveralls.io/github/sudaning/Fpcap
[license-badge]:   https://img.shields.io/badge/license-MIT-007EC7.svg

