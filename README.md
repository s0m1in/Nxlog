# Nxlog
#At C:\Program Files (x86)\nxlog

define ROOT C:\Program Files (x86)\nxlog

Moduledir %ROOT%\modules
CacheDir %ROOT%\data
Pidfile %ROOT%\data\nxlog.pid
SpoolDir %ROOT%\data
LogFile %ROOT%\data\nxlog.log

<Extension json>
    Module	xm_json
</Extension>

<Input eventlog>
    Module      im_msvistalog
    Query       <QueryList>\
                    <Query Id="0">\
                        <Select Path="Application">*</Select>\
                        <Select Path="System">*</Select>\
                        <Select Path="Security">*</Select>\
                    </Query>\
                </QueryList>
    Exec	to_json();
</Input>

<Output out>
    Module      om_tcp
    Host        192.168.10.41
    Port        9527
</Output>

<Route 1>
    Path        eventlog => out
</Route>
