## How to chain hooks call

In Watch page, first we fetch the video details and then from that we pick the comedian id and then get the recommendations.

Both calls use useCocoApi hook. But for the second hook call to wait for the first call to complete, we are creating a new component which calls the useCocoApi once the details from first call is available as props.

## References

- How react hooks works (https://www.netlify.com/blog/2019/03/11/deep-dive-how-do-react-hooks-really-work/)

- How to use swr and its features (https://www.smashingmagazine.com/2020/06/introduction-swr-react-hooks-remote-data-fetching/)
