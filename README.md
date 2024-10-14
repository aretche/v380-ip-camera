# Cámara IP NewVision DC74 

Material e información sobre la cámara IP [NewVision DC74](https://www.newvision.com.ar/producto/camara-seguridad-ip-wifi-inalambrica-mini-newvision/) ([Anyka AK3918 V200](https://monitor.espec.ws/files/700cafec77419c2d7705f376c974b8d0ff72_986.pdf) - [V380](https://v380.org/))



## Servicios disponibles:

### RTSP

- Video h264 640x360 20 fps: `rtsp://X.X.X.X/live/ch00_0`
- Video h264 1280x720 20 fps: `rtsp://X.X.X.X/live/ch00_1`

Por ejemplo para ver el stream de video HD en ffmpeg:

```bash
ffplay -rtsp_transport tcp rtsp://X.X.X.X/live/ch00_1
```

Reemplazar `X.X.X.X` por la dirección IP de la cámara.

### ONVIF

La cámara expone via gSOAP los servicios ONVIF en el puerto 8899.

Los servicios más interesantes son _obtener el stream de video_ y _mover la cámara_ (PTZ).

Según [blog.caller.xyz](https://blog.caller.xyz/v380-ipcam-move-with-soap/) los servicios disponibles son:

* ContinuousMove
* GetAudioEncoderConfiguration
* GetAudioEncoderConfigurations
* GetAudioSourceConfiguration
* GetAudioSourceConfigurations
* GetAudioSources
* GetCapabilities
* GetConfiguration
* GetConfigurationOptions
* GetConfigurations
* GetDeviceInformation
* GetOptions
* GetProfile
* GetProfiles
* GetServices
* GetStreamUri
* GetSystemDateAndTime
* GetVideoEncoderConfiguration
* GetVideoEncoderConfigurationOptions
* GetVideoEncoderConfigurations
* GetVideoSourceConfiguration
* GetVideoSourceConfigurations
* GetVideoSources
* SetVideoEncoderConfiguration
* Stop

Por ejemplo para mover la camara se pueden usar estos dos comandos cURL:

```bash
curl http://X.X.X.X:8899/ \
  -H 'Content-Type: application/soap+xml;charset=UTF-8;action="http://www.onvif.org/ver20/ptz/wsdl/ContinuousMove"' \
  --data '<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope" xmlns:wsdl="http://www.onvif.org/ver20/ptz/wsdl" xmlns:sch="http://www.onvif.org/ver10/schema">
   <soap:Header/>
   <soap:Body>
      <wsdl:ContinuousMove>
         <wsdl:ProfileToken>blah</wsdl:ProfileToken>
         <wsdl:Velocity>
            <sch:PanTilt x="-0.1" y="0.1" space="0.3"/>
            <sch:Zoom x="0" space="0"/>
         </wsdl:Velocity>
      </wsdl:ContinuousMove>
   </soap:Body>
</soap:Envelope>'
curl http://X.X.X.X:8899/ \
  -H 'Content-Type: application/soap+xml;charset=UTF-8;action="http://www.onvif.org/ver20/ptz/wsdl/Stop"' \
  --data '<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope" xmlns:wsdl="http://www.onvif.org/ver20/ptz/wsdl">
   <soap:Header/>
   <soap:Body>
      <wsdl:Stop>
         <wsdl:ProfileToken>whatevs</wsdl:ProfileToken>
         <wsdl:PanTilt>1</wsdl:PanTilt>
         <wsdl:Zoom>1</wsdl:Zoom>
      </wsdl:Stop>
   </soap:Body>
</soap:Envelope>'
```

Reemplazar `X.X.X.X` por la dirección IP de la cámara.

Los WSDL de los servicios ONVIF están disponibles en la cámara en la URL `http://X.X.X.X:8899/onvif/device_service`.

## Recursos

- https://ricardojlrufino.wordpress.com/2022/02/14/hack-ipcam-anyka-teardown-and-root-access/
- https://gitea.raspiweb.com/Gerge/Anyka_ak3918_hacking_journey
- [SecTalks London 0x11 - Introduction to Hardware Hacking](docs/SecTalks_London_0x11-Hardware_Workshop.pdf)

### Techical Documentation

- [AK3918 HD IP Camera SoC Specification](docs/AK3918_HD_IP_Camera_SoC-specs.pdf)
- [H155E-U Wi-Fi Single-band 1x1 802.11b/g/n USB Module Datasheet](docs/H155E-U_SV6155P_wifi-module-specs.pdf)

### Firmware

- [https://github.com/MuhammedKalkan/Anyka-Camera-Firmware](https://github.com/MuhammedKalkan/Anyka-Camera-Firmware)
- 