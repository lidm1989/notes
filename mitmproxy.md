# chcp 437
# pip install mitmproxy
# pip install protobuf==3.3.0

# mitmdump.py

	#!/usr/bin/python

	# -*- coding: utf-8 -*-
	import re
	import sys

	sys.path.append(".")

	import shterm
	from mitmproxy.tools.main import mitmdump

	if __name__ == '__main__':
	    t = shterm.Transfer()
	    t.setDaemon(True)
	    t.start()

	    sys.argv[0] = re.sub(r'(-script\.pyw?|\.exe)?$', '', sys.argv[0])
	    sys.exit(mitmdump())
