
The SSH2 WebRTC client library used on the sqs.io page. 

### using

```html
<script src="/js/RtcSSH2.js"></script>
```
### example
```html
<script>
rtc = new Stream.RtcNegotiation()
	rtc.init({
		remoteKey: '<uuid>',
		signallingServer: '<wss://...>'
		port: 443
	})
	rtc.on('createRTCPeerConnection', function() {
		rtc.createData('SSH', function(rtcChannel) {
			ssh2 = new Stream.SSH2()
			ssh2.init({   
				sock: Stream.RtcStream(rtcChannel),
				username: '<login>',
				password: '<password>'
			})
			ssh2.on('error', (e) => {
				console.log(e.message)
			})
			ssh2.on('ready', () => {
				ssh2.shell({term: 'xterm-256color',rows:24, cols:80},(err, channel) => {
					channel.write("ls -l")       	 // example send command
					channel.on('data', (data) => {
						console.log(data.toString())  // receive 
					})
					channel.on('close', ()=> {
						ssh2.end()
					})
				})
			})
		})
	
	})	
	rtc.on('error', (err) => {
		console.log(err)
	})
	rtc.on('disconnected', ()=> {
		console.log("RTC disconnectet")
	})
</script>
```
