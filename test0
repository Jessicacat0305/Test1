"# Test1" 
# -*- coding: utf-8 -*-
from linepy import *
from datetime import datetime
import codecs, json, time, timeit
cl = LINE()
oepoll = OEPoll(cl)
settingsOpen = codecs.open("temp.json","r","utf-8")
settings = json.load(settingsOpen)
clMID = cl.profile.mid
wait2 = {
    'readPoint':{},
    'readMember':{},
    'setTime':{}
}
setTime,msg_dict,admin = wait2['setTime'],{},[clMID]
del settingsOpen
def backupData():
    try:
        backup = settings
        f = codecs.open('temp.json','w','utf-8')
        json.dump(backup, f, sort_keys=True, indent=4, ensure_ascii=False)
        return True
    except Exception as error:
        logError(error)
        return False
def logError(text):
    cl.log("[ ERROR ] " + str(text))
def helpmessage():
    helpMessage = """
°o‧°  °o‧° 指令表 °o‧° °o‧°  
°o‧°→ 「Speed」查看速度
°o‧°→ 「Set」查看設定
°o‧°→ 「Reread On/Off」查看收回
°o‧°→ 「Delrp」刪除已讀點
°o‧°→ 「Setrp」設定已讀點
°o‧°→ 「Lookrp」查詢已讀點
°o‧°→ 「Tagall」標註全部人
°o‧°  °o‧°  °o‧° °o‧° °o‧° 
"""
    return helpMessage
def lineBot(op):
    try:
        if op.type == 0:
            return
        if op.type in [25,26]:
            msg = op.message
            text = msg.text
            if msg.toType == 0:
                if msg._from != cl.profile.mid:
                    to = msg._from
                else:
                    to = msg.to
            else:
                to = msg.to
            if msg.contentType == 0:
                if text is None:
                    return
            if msg._from in admin:
                if text.lower() == 'help':
                    helpMessage = helpmessage()
                    cl.sendMessage(to, str(helpMessage))
                elif msg.text in ["SR","Setrp"]:
                    cl.sendMessage(to, "設置已讀點")
                    wait2['readPoint'][msg.to] = msg.id
                    wait2['readMember'][msg.to] = ""
                    wait2['setTime'][msg.to] = datetime.strftime(datetime.now(),"%H:%M")
                elif msg.text in ["DR","Delread"]:
                    cl.sendMessage(to, "刪除已讀點")
                    try:
                        del wait2['readPoint'][msg.to]
                        del wait2['readMember'][msg.to]
                        del wait2['setTime'][msg.to]
                    except:
                        pass
                elif msg.text in ["LR","Lookrp"]:
                    if msg.to in wait2['readPoint']:
                        cl.sendMessage(to, "||已讀的人||%s\n[%s]" % (wait2['readMember'][msg.to],setTime[msg.to]))
                    else:
                        cl.sendMessage(to, "請輸入SR設置已讀點")
                elif text.lower() == 'speed':
                    time0 = timeit.timeit('"-".join(str(n) for n in range(100))', number=10000)
                    str1 = str(time0)
                    start = time.time()
                    cl.sendMessage(to,'處理速度\n' + str1 + '秒')
                    elapsed_time = time.time() - start
                    cl.sendMessage(to,'指令反應\n' + format(str(elapsed_time)) + '秒')
                elif text.lower() == 'set':
                    ret = "°o‧°  °o‧° 指令表 °o‧° °o‧°"
                    if settings["reread"] == True: ret += "\n╠ 查詢收回開啟 ?"
                    else: ret += "\n╠ 查詢收回關閉 ?"
                    ret += "\n°o‧°  °o‧° 指令表 °o‧° °o‧°"
                    cl.sendMessage(to, str(ret))
                elif text.lower() == 'reread on':
                    settings["reread"] = True
                    cl.sendMessage(to, "查詢收回開啟")
                elif text.lower() == 'reread off':
                    settings["reread"] = False
                    del msg_dict
                    msg_dict = {}
                    cl.sendMessage(to, "查詢收回關閉")
                elif text.lower() == 'tagall':
                    group = cl.getGroup(msg.to)
                    nama = [contact.mid for contact in group.members]
                    k = len(nama)//100
                    for a in range(k+1):
                        txt = u''
                        s=0
                        b=[]
                        for i in group.members[a*100 : (a+1)*100]:
                            b.append({"S":str(s), "E" :str(s+6), "M":i.mid})
                            s += 7
                            txt += u'@Fuck \n'
                        cl.sendMessage(to, text=txt, contentMetadata={u'MENTION': json.dumps({'MENTIONEES':b})}, contentType=0)
                        cl.sendMessage(to, "總共 {} 個成員".format(str(len(nama))))
        if op.type == 26:
            try:
                msg = op.message
                if settings["reread"] == True:
                    if msg.toType == 0:
                        cl.log("[%s]"%(msg._from)+msg.text)
                    else:
                        cl.log("[%s]"%(msg.to)+msg.text)
                    if msg.contentType == 0:
                        msg_dict[msg.id] = {"text":msg.text,"from":msg._from}
                    if len(msg_dict) > 50 :
                        del msg_dict[min(list(msg_dict.keys()))]
                else:
                    pass
            except Exception as e:
                logError(e)
        if op.type == 65:
            try:
                at = op.param1
                msg_id = op.param2
                if settings["reread"] == True:
                    if msg_id in msg_dict:
                        if msg_dict[msg_id]["from"] not in [""]:
                            cl.sendMessage(at,"[收回訊息者]\n%s\n[訊息內容]\n%s"%(cl.getContact(msg_dict[msg_id]["from"]).displayName,msg_dict[msg_id]["text"]))
                        del msg_dict[msg_id]
                else:
                    pass
            except Exception as e:
                logError(e)
        if op.type == 55:
            try:
                if op.param1 in wait2['readPoint']:
                    Name = cl.getContact(op.param2).displayName
                    if Name in wait2['readMember'][op.param1]:
                        pass
                    else:
                        wait2['readMember'][op.param1] += "\n[‧]" + Name
                else:
                    pass
            except:
                pass
    except Exception as error:
        logError(error)
while True:
    try:
        ops = oepoll.singleTrace(count=50)
        if ops is not None:
            for op in ops:
                lineBot(op)
                oepoll.setRevision(op.revision)
    except Exception as e:
        logError(e)

