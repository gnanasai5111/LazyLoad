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
