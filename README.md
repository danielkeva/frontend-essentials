# React Components

## If

Is meant to replace short-circuit evaluation conditional rendering:

Instead of:

    {userLoggedIn && !loading && <UserModal />}

Do:

    <If condition={userLoggedIn && !loading}>
        <UserModal />
    </If>

## LazyRender

Lazily renders a large list of items:

    <LazyRender items={notes} batch={20}>
        {({ _id, title, content, date }) => (
            <Note
                key={_id}
                title={title}
                content={content}
                date={date}
            />
        )}
    </LazyRender>

## Media

Conditionally renders a component when certain media queries are matched:

    <Media query="(max-width: 768px)">
        <Menu />
    </Media>

When an array is passed to `query`, queries will have an `OR` relationship when matching:

    // matches when width is at most 480px or at least 1200px
    <Media query=["(max-width: 480px)", "(min-width: 1200px)"]>
        <Menu />
    </Media>

# React Hooks

## useAxios

Handles async requests easily:

    const { loading, status, error, data, activate } = useAxios({
        method: 'get',
        url: 'https://example.com/v1/items',
        onSuccess: ({ data }) => console.log(data)
    })

Initial request can be skipped and triggered manually:

    const { loading, status, error, data, activate } = useAxios({
        method: 'get',
        url: 'https://example.com/v1/items',
        manual: true,
        onError: ({ error }) => console.error(error)
    })

    setTimeout(activate, 3000)

Get axios instance:

    import { axios } from 'frontend-essentials'

    axios.defaults.withCredentials = true

## useProgressiveImage

Returns a low quality image source until a higher resolution image is fetched:

    const [src, blur] = useProgressiveImage(lowQualitySrc, highQualitySrc)

    return <img style={{filter: blur ? 'blur(20px)' : 'none'}} src={src} />

## useViewport

Returns a viewport object containing boolean matches for each passed media query:

    const { mobile, tablet } = useViewport({
        mobile: '(max-width: 480px)',
        tablet: '(min-width: 481px) and (max-width: 1199px)'
    })

    const getDescription = () => {
    	if (mobile) return 'Hello'
    	if (tablet) return 'Hello Mr.'
    	return 'Hello Mr. Smith'
    }

    return <div>{getDescription()}</div>
