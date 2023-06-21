# Third Party Cookies Detection

How to use: 

```
import { useEffect, useState } from 'react';

const useCheckThirdPartyCookies = () => {
    const [thirdPartyCookieSupported, setThirdPartyCookieSupported] = useState(false);

    useEffect(() => {
        // creating an iframe
        const frame = document.createElement("iframe");
        frame.id = "check_3rd_party_cookie";
        frame.src = "<custom_domain>"; // fork this repo
        frame.style.display = "none";
        frame.style.position = "fixed";
        document.body.appendChild(frame);

        const supported = "Third party cookie supported!"
        const unsupported = "Third party cookie not supported!"

        // listen to message
        window.addEventListener(
            "message",
            function listen(event) {
                if (event.data.includes([supported, unsupported])) {
                    setThirdPartyCookieSupported(event.data === supported);
                    document.body.removeChild(frame); // removing the iframe
                    window.removeEventListener("message", listen); // clean up the listener
                }
            },
            false
        );
    }, []);

    return thirdPartyCookieSupported;
};

export default useCheckThirdPartyCookies;
```

