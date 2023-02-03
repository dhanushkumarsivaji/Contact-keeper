
## Usage

Install dependencies

```bash
npm install
npm client-install
```

### Mongo connection setup

Edit your /config/default.json file to include the correct MongoDB URI

### Run Server

```bash
npm run dev     # Express & React :3000 & :5000
npm run server  # Express API Only :5000
npm run client  # React Client Only :3000
```

```
import { useEffect, useRef } from "react";
import { useDispatch } from "react-redux";
import { getCurrentTime, getLocalValue } from "./textUtility";
import { clearSessionData } from "./SessionUtility";
import { LoggedOut, fetchRefreshToken } from "../redux/auth/actions";
import { apiErrorMessage } from "../redux/global/actions";
import { CHECK_EXPIRATION_TIME_INTERVAL } from "../constants";

export const useCheckExpirationTime = () => {
  let dispatch = useDispatch();
  let interval = useRef(null);

  useEffect(() => {
    interval.current = setInterval(
      () => checkExpirationTime(),
      CHECK_EXPIRATION_TIME_INTERVAL
    );
    return () => clearInterval(interval.current);
    // eslint-disable-next-line
  }, []);

  const checkExpirationTime = async () => {
    const {
      accessTokenExpirationTime,
      refreshTokenExpirationTime,
    } = sessionStorage;
    let refreshTokenValue = getLocalValue("refreshToken");
    if (refreshTokenExpirationTime && accessTokenExpirationTime) {
      let currentTime = await getCurrentTime();
      if (
        currentTime > accessTokenExpirationTime &&
        currentTime <= refreshTokenExpirationTime
      ) {
        let token = { refreshToken: refreshTokenValue };
        dispatch(fetchRefreshToken(token));
      } else if (
        currentTime > refreshTokenExpirationTime &&
        currentTime > accessTokenExpirationTime
      ) {
        clearSessionData();
        dispatch(LoggedOut(true));
        dispatch(apiErrorMessage("API_UNAUTHORIZED_ERROR"));
      }
    }
  };
};

```
