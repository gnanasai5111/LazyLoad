```

import { useEffect, useRef, useState } from "react";

const LazyImage = ({ src, alt, className }) => {
  const imgRef = useRef(null);
  const [imgSrc, setImgSrc] = useState();
  useEffect(() => {
    const handleScroll = () => {
      if (
        imgRef.current &&
        imgRef.current.getBoundingClientRect().top <= window.innerHeight
      ) {
        setImgSrc(src);
      }
    };
    console.log(imgRef);
    handleScroll();
    window.addEventListener("scroll", handleScroll);
    return () => window.removeEventListener("scroll", handleScroll);
  }, []);

  if (!imgSrc) {
    return <div className="skeleton" ref={imgRef}></div>;
  }
  return <img src={imgSrc} alt={alt} className={className} />;
};

export default LazyImage;


```

```

import { useEffect, useRef, useState } from "react";

const LazyImage = ({ src, alt, className }) => {
  const imgRef = useRef(null);
  const [imgSrc, setImgSrc] = useState(
    "https://img.freepik.com/premium-photo/white-grey-gradient-wall-banner-blank-studio-room-interior-present-product_28629-598.jpg?w=2000"
  );

  const [isVisible, setIsVisible] = useState(false);
  useEffect(() => {
    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting && entry.intersectionRatio === 1) {
          setImgSrc(src);
          observer.disconnect();
        }
      },
      {
        root: null,
        rootMargin: "0px",
        threshold: 1, // Fully visible
      }
    );
    const imgElement = imgRef.current;
    if (imgElement) {
      observer.observe(imgElement);
    }
    return () => observer.unobserve(imgElement);
  }, []);

  const handleLoad = () => {
    setIsVisible(true);
  };

  return (
    <>
      <img
        src={imgSrc}
        alt={alt}
        onLoad={handleLoad}
        ref={imgRef}
        className={className}
      />
    </>
  );
};

export default LazyImage;


```
